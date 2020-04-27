Toic -> partition->segment
Segment like 1 GB or 1 week of data

Zookeeper maintaain list of brokers
Controller perform routine monitoring activies. Only one broker is elected at a controller at a time.

All R/W operations are done by Leader
Followers are responsible to take copy of Leader for high availability.


ISR - In Sync Replica

###min insync replica
Set min replicas on queue. It ensure that the message is commited to given minimum number of replicas before it is considered as commited.
%KAFKA_HOME%\bin\windows\kafka-topics.bat --alter --zookeeper localhost:2181 --topic test --config min.insync.replicas=2
Leader is included in the given number.

Set ack on producer  
kafka-console-producer.bat --broker-list localhost:9092 --topic  temp --request-required-acks all
The producer will wait for the acknowledge from leader that the given message is commited to all replicas. If available replica is less than min.insync replicas then message is rejected.


### Message Delivery
## Atlest once delivery
## At Most once delivery
	Set retries=0 at producer.
## Exactly once delivery
	set enable.idempotence=true on producer. 
	Idempotence property automatically set to true when transaction is enabled.
	Broker assign a sequence number to each message.


##how to tell consumer to read from last message it read earlier.
properties.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, "true");
Zookeeper stored last committed offset for the given client-group if above property set to true. Current offset becomes this value when consumer restarts.
If this value is false, consumer should cpmmit batch before next poll.


####
callback-producer
	   producer.send(..., (recordMetadata, e)->{});
sync-producer
		Metadata = producer.send(...).get();
partitioned-producer
		props.put(ProducerConfig.PARTITIONER_CLASS_CONFIG, OddEvenPartitioner.class.getName());
		
## At consumer end
Current offset and commit offset position
auto-offset-reset values are earlier or latest.
enable-auto-commit default value is true.		

## Stream topology
Step by step computational logic of a stream processing application.
KStream - abstraction of a stream of kafka message records.  Methods filter(), map(), flatmap() foreach to()

## processors
	stateless -  filter (), map(),  foreach(), to(), peek()
					through() - Through processor seemlessly reparition the message through a intermediate topic.
	stateful -   join(), transformValues(), groupby(),mapValues(), flatMapValues(), groupByKey() , aggregate()

###
Key preserving API
	mapValues(), flatMapValues(), groupByKey()
Key changing API
	map(), flatMap(), groupBy()
	
##Store types
	KeyValueStore, SessionStore, WindowStore
	
	
## Ktable and GlobalKTable
To execute queries	


##Tumbling Window
Fixed size, non overlapping, gap-less windows are called tumbling window.

##Hopping window	
	fixed size overlapping window gap-less window.
	
	
##Session window
	window wih gaps..
	
##Joins
KStream-to-KStream	: 		always windowed join.
KTable to KTable :  non -window join


###Testing
##Unit Testing  - use TopologyTestDriver
	Allows to input data, run topology, read final outcome and assert outcome.
	
##Integration testing - use embedded Kafka Cluster