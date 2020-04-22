set KAFKA_HOME=D:\sunil\kafka\confluent-5.4.1

###
%KAFKA_HOME%\bin\windows\zookeeper-server-start.bat %KAFKA_HOME%\etc\kafka\zookeeper.properties
%KAFKA_HOME%\bin\windows\kafka-server-start.bat %KAFKA_HOME%\etc\kafka\server-0.properties


###zookeeper shell
%KAFKA_HOME%\bin\windows\zookeeper-shell.bat localhost:2181
ls /
ls /brokers/ids


# create topic
%KAFKA_HOME%\bin\windows\kafka-topics.bat --create --topic ticks2 --partitions 2 --replication-factor 3 --bootstrap-server localhost:9092


###  describe topic
%KAFKA_HOME%\bin\windows\kafka-topics.bat --describe --topic ticks2 --bootstrap-server localhost:9092

###min insync repl8ca
Set min replicas on queue. It ensure that the message is commited to given minimum number of replicas before it is commited.
%KAFKA_HOME%\bin\windows\kafka-topics.bat --alter --zookeeper localhost:2181 --topic ticks --config min.insync.replicas=2
Leader is included in the given number.

Set ack on producer  [properties.put(ProducerConfig.ACKS_CONFIG, "all");]
The producer will wait for the acknowledge from leader that the given message is commited to all replicas.








#### producer consumer example
# https://www.tutorialkart.com/apache-kafka/kafka-console-producer-and-consumer-example/


%KAFKA_HOME%\bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic  test1
%KAFKA_HOME%\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test1 --from-beginning



### connector
%KAFKA_HOME%\bin\windows\connect-standalone.bat %KAFKA_HOME%\etc\kafka\connect-standalone.properties %KAFKA_HOME%\etc\kafka\connect-file-source.properties etc\kafka\connect-file-sink.properties

###run a java program
mvn -q clean compile exec:java -Dexec.mainClass="guru.learningjournal.kafka.examples.HelloProducer"


### consumer-group
%KAFKA_HOME%\bin\windows\kafka-topics.bat --create --topic stock-ticks --partitions 3 --replication-factor 1 --bootstrap-server localhost:9092
%KAFKA_HOME%\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic stock-ticks --from-beginning --group stock-ticks-group
%KAFKA_HOME%\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic stock-ticks --from-beginning --group stock-ticks-group
%KAFKA_HOME%\bin\windows\kafka-dump-log.bat --files D:\tmp\kafka-logs-0\stock-ticks2-1\00000000000000000000.log
%KAFKA_HOME%\bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic  stock-ticks

### Get last message
%KAFKA_HOME%\bin\windows\kafka-run-class.bat kafka.tools.GetOffsetShell --broker-list localhost:9092 --topic topic10 
%KAFKA_HOME%\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic testsegment --offset 10 --partition 0






