# Event Registry - Application Events können nicht verschickt werden

Beim Versenden von Events mit Templates erhält man einen Fehler, dass die column serialized_event zu klein ist.

## Code

### Stack Trace
```
java.sql.SQLSyntaxErrorException: (conn=1026) Data too long for column 'serialized_event' at row 1
	at org.mariadb.jdbc.export.ExceptionFactory.createException(ExceptionFactory.java:289) ~[mariadb-java-client-3.4.1.jar:na]
	at org.mariadb.jdbc.export.ExceptionFactory.create(ExceptionFactory.java:378) ~[mariadb-java-client-3.4.1.jar:na]
	at org.mariadb.jdbc.message.ClientMessage.readPacket(ClientMessage.java:189) ~[mariadb-java-client-3.4.1.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.readPacket(StandardClient.java:1235) ~[mariadb-java-client-3.4.1.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.readResults(StandardClient.java:1174) ~[mariadb-java-client-3.4.1.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.readResponse(StandardClient.java:1093) ~[mariadb-java-client-3.4.1.jar:na]
	at org.mariadb.jdbc.client.impl.StandardClient.execute(StandardClient.java:1017) ~[mariadb-java-client-3.4.1.jar:na]
	at org.mariadb.jdbc.ClientPreparedStatement.executeInternal(ClientPreparedStatement.java:101) ~[mariadb-java-client-3.4.1.jar:na]
```

## Ursache

Die Datenbank speichert `serialized_events` als `VARCHAR` ab, wodurch es bei größeren Metadaten zu Fehlern kommt. 

## Lösung

Den Datentypen auf Mediumtext mithilfe einer Datenbankmigration von Liquibase verändern. Da Spring die Event-Tabelle dynamisch erstellt, kann es passieren, dass sie beim ersten Start noch gar nicht existiert. Die Migration muss deshalb zwei Dinge abdecken:
- Wenn die Tabelle noch nicht da ist, soll sie inklusive einer serialized_event-Spalte vom Typ `MEDIUMTEXT` neu angelegt werden.
- Wenn die Tabelle bereits existiert, reicht es, nur den Typ der Spalte serialized_event entsprechend zu ändern – alles andere soll dabei unberührt bleiben.

### Code
```yaml
- changeSet:
  id: alter_event_publication_table_columns
  author: 
  context: local,dev,prod,test
  runAlways: true
  changes:
	- sqlFile:
		path: db/changelog/alter_event_publication_table_column_sizes.sql
```

```sql
-- Create the table if it does not exist
CREATE TABLE IF NOT EXISTS event_publication (
    completion_date DATETIME(6),
    publication_date DATETIME(6),
    id VARCHAR(255),
    event_type VARCHAR(255),
    listener_id VARCHAR(255),
    serialized_event MEDIUMTEXT,
    PRIMARY KEY (id)
    );

-- Alter the attributes of the table
ALTER TABLE event_publication
    MODIFY COLUMN completion_date DATETIME(6),
    MODIFY COLUMN publication_date DATETIME(6),
    MODIFY COLUMN id VARCHAR(255),
    MODIFY COLUMN event_type VARCHAR(255),
    MODIFY COLUMN listener_id VARCHAR(255),
    MODIFY COLUMN serialized_event MEDIUMTEXT;
```