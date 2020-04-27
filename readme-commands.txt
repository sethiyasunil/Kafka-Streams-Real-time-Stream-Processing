
set KAFKA_HOME=D:\sunil\kafka\confluent-5.4.1

###
%KAFKA_HOME%\bin\windows\zookeeper-server-start.bat %KAFKA_HOME%\etc\kafka\zookeeper.properties
%KAFKA_HOME%\bin\windows\kafka-server-start.bat %KAFKA_HOME%\etc\kafka\server-0.properties
%KAFKA_HOME%\bin\windows\zookeeper-shell.bat localhost:2181
ls /
ls /brokers/ids


# create topic
%KAFKA_HOME%\bin\windows\kafka-topics.bat --create --topic ad-clicks --partitions 2 --replication-factor 2 --bootstrap-server localhost:9092


%KAFKA_HOME%\bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic  temp --property "parse.key=true" --property "key.separator=:"
%KAFKA_HOME%\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic temp --from-beginning  --group x --property print.key=true --property key.separator="-" 
%KAFKA_HOME%\bin\windows\kafka-topics.bat --describe --bootstrap-server localhost:9092 --topic test
%KAFKA_HOME%\bin\windows\kafka-topics.bat --zookeeper localhost:2181 --delete --topic employees
%KAFKA_HOME%\bin\windows\kafka-topics.bat --list --zookeeper localhost:2181

%KAFKA_HOME%\bin\windows\kafka-dump-log.bat --files D:\tmp\kafka-logs-0\stock-ticks2-1\00000000000000000000.log

### Get last message
%KAFKA_HOME%\bin\windows\kafka-run-class.bat kafka.tools.GetOffsetShell --broker-list localhost:9092 --topic topic10 
%KAFKA_HOME%\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic testsegment --offset 10 --partition 0


### connector
%KAFKA_HOME%\bin\windows\connect-standalone.bat %KAFKA_HOME%\etc\kafka\connect-standalone.properties %KAFKA_HOME%\etc\kafka\connect-file-source.properties etc\kafka\connect-file-sink.properties









