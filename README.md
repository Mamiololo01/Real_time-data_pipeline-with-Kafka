# Real_time-data_pipeline-with-Kafka
Implement real time data streaming application using Kafka, AWS Athena, Glue and S3 bucket.



In this project, you will execute an End-To-End Data Engineering Project on Real-Time Stock Market Data using Kafka.
We are going to use different technologies such as Python, Amazon Web Services (AWS), Apache Kafka, Glue, Athena, and SQL.

## Design

![Architecture](https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/4f00a623-8343-47c0-a8e9-708f6d11b220)
￼

## Technology Used
Programming Language - Python
Amazon Web Service (AWS)
1. S3 (Simple Storage Service)
2. Athena
3. Glue Crawler
4. Glue Catalog
5. EC2
Apache Kafka including installing Java runtime.

## Basic and core concepts of Kafka.

DistributedKafka works as a cluster of one or more nodes that can live in different Datacenters, we can distribute data/ load across different nodes in the Kafka Cluster, and it is inherently scalable, available, and fault-tolerant.

Streaming Platform
Kafka stores data as a stream of continuous records which can be processed in different methods.

Commit Log

This one is my favorite. When you push data to Kafka it takes and appends them to a stream of records, like appending logs in a log file or if you’re from a Database background like the WAL. This stream of data can be “Replayed” or read from any point in time.

Is Kafka a message queue?

It certainly can act as a message queue, but it’s not limited to that. It can act as a FIFO queue, as a Pub/ Sub messaging system, a real-time streaming platform. And because of the durable storage capability of Kafka, it can even be used as a Database.

Having said all of that, Kafka is commonly used for real-time streaming data pipelines, i.e. to transfer data between systems, building systems that transform continuously flowing data, and building event-driven systems.

### Message

A message is the atomic unit of data for Kafka. Let’s say that you are building a log monitoring system, and you push each log record into Kafka, your log message is a JSON that has this structure.

When you push this JSON into Kafka you are actually pushing 1 message. Kafka saves this JSON as a byte array, and that byte array is a message for Kafka. This is that atomic unit, a JSON having two keys “level” and “message”. But it does not mean you can’t push anything else into Kafka, you can push String, Integer, a JSON of different schema, and everything else, but we generally push different types of messages into different topics (we will get to know what is a topic soon).

Messages might have an associated “Key” which is nothing but some metadata, which is used to determine the destination partition (will know soon as well) for a message.

### Topic

Topics, as the name suggests, are the logical categories of messages in Kafka, a stream of the same type of data. Going back to our previous example of the logging system, let’s say our system generates application logs, ingress logs, and database logs and pushes them to Kafka for other services to consume. Now, these three types of logs can be logically be divided into three topics, appLogs, ingressLogs, and dbLogs. We can create these three topics in Kafka, whenever there’s an app log message, we push it to appLogs topic and for database logs, we push it to the dbLogs topic. This way we have logical segregation between messages, sort of like having different tables for holding different types of data.

### Partitions

Partition is analogous to shard in the database and is the core concept behind Kafka’s scaling capabilities. Let’s say that our system becomes really popular and hence there are millions of log messages per second. So now the node on which appLogs topic is present, is unable to hold all the data that is coming in. We initially solve this by adding more storage to our node i.e. vertical scaling. But as we all know vertical scaling has its limit, once that threshold is reached we need to horizontally scale, which means we need to add more nodes and split the data between the nodes. When we split data of a topic into multiple streams, we call all of those smaller streams the “Partition” of that topic.

### Producer

A producer is the Kafka client that publishes messages to a Kafka topic. Also one of the core responsibilities of the Producer is to decide which partition to send the messages to. Depending on various configuration and parameters, the producer decides the destination partition, let’s look a bit more into this.
* 		No Key specified => When no key is specified in the message the producer will randomly decide partition and would try to balance the total number of messages on all partitions.
* 		Key Specified => When a key is specified with the message, then the producer uses Consistent Hashing to map the key to a partition. Don’t worry if you don’t know what consistent hashing is, in short, it’s a hashing mechanism where for the same key same hash is generated always, and it minimizes the redistribution of keys on a re-hashing scenario like a node add or a node removal to the cluster. So let’s say in our logging system we use source node ID as the key, then the logs for the same node will always go to the same partition. This is very relevant for the order guarantees of messages in Kafka, we will shortly see how.
* 		Partition Specified => You can hardcode the destination partition as well.
* 		Custom Partitioning logic => We can write some rules depending on which the partition can be decided.
* 		
Consumer

So far we have produced messages, to read those messages we use Kafka consumer. A consumer reads messages from partitions, in an ordered fashion. So if 1, 2, 3, 4 was inserted into a topic, the consumer will read it in the same order. Since every message has an offset, every time a consumer reads a message it stores the offset value onto Kafka or Zookeeper, denoting that it is the last message that the consumer read. So in case, a consumer node goes down, it can come back and resume from the last read position. Also if at any point in time a consumer needs to go back in time and read older messages, it can do so by just resetting the offset position.

### Consumer Group

A consumer group is a collection of consumers that work together to read messages from a topic. There are some very interesting concepts here, let’s go through them.
* 		Fan out exchange => A single topic can be subscribed to by multiple consumer groups. Let’s say that you are building an OTP service.

Now you need to send both text and email OTP. So your OTP service can put the OTP in Kafka, and then the SMS Service consumer group and Email Service consumer group can both receive the message and can then send the SMS and email out.

Order guarantee => Now we have seen that a topic can be partitioned and multiple consumers can consumer from the same topic, then how do you maintain the order of messages on the consumer-end one might ask. Good question. One partition can not be read by multiple consumers in the same consumer group. This is enabled by the consumer group only, only one consumer in the group gets to read from a single partition. 

### Broker

A broker is a single Kafka server. Brokers receive messages from producers, assigns offset to them, and then commit them to the partition log, which is basically writing data to disk, and this gives Kafka its durable nature.

### Cluster

A Kafka cluster is a group of broker nodes working together to provide, scalability, availability, and fault tolerance. One of the brokers in a cluster works as the Controller, which basically assigns partitions to brokers, monitors for broker failure to do certain administrative stuff.
In a cluster, partitions are replicated on multiple brokers depending on the replication factor of the topic to have failover capability. What I mean is, for a topic of replication factor 3, each partition of that topic will live onto 3 different brokers. When a partition is replicated onto 3 brokers, one of the brokers will act as the leader for that partition and the rest two will be followers. Data is always written on the leader broker and then replicated to the followers. This way we do not lose data nor availability of the cluster, and if the leader goes down another leader is elected

### Zookeeper

Kafka does not function without zookeeper( at least for now, they have plans to deprecate zookeeper in near future). Zookeeper works as the central configuration and consensus management system for Kafka. It tracks the brokers, topics, and partition assignment, leader election, basically all the metadata about the cluster.

And these my friend were the basic and core concepts of Kafka.


### Procedures
Install Apache Kafka on EC2 by installing an EC2 instance and install Zookeeper, KafkaConsumer and KafkaProducer on the ec2 instance on AWS. Kindly ensure the instance-type has enough resources for the Apache Kafka. You will need to open 3 differenet VM tabs for the dependencies.

<img width="943" alt="Screenshot 2023-09-17 at 21 24 48" src="https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/bc437fc4-2468-442a-9f91-e37d674601fb">


Create an S3 bucket for data ingestion.

<img width="796" alt="Screenshot 2023-09-17 at 21 28 00" src="https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/5c1c1250-d2b1-4625-a9e3-87f581b7698a">


Configure a lambda function, use runtime as python and add the python code on the configuration tab and deploy.


Create AWS Glue and crawler for the file on the s3 bucket and run the query on AWS Athena. Create crawler and add data source as s3, choose the s3 bucket as the file path.. Please ensure there is IAM role for AWS crawler to have access to S3  bucket. Finally create a database for the datawarehoue for the analysis will happen with the Athena.

Create another S3 bucket for Athena temporary queries and must be in the same region as the s3 bucket for the ingestion.


<img width="1252" alt="Screenshot 2023-08-13 at 22 25 55" src="https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/ed5f8c42-d619-49fd-a7cd-af766098e6c2">

<img width="1275" alt="Screenshot 2023-08-13 at 22 26 17" src="https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/196d3665-3237-439d-9714-876c6b3d6ff3">

<img width="1253" alt="Screenshot 2023-08-13 at 22 38 19" src="https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/f3c67fbb-81d3-43f0-906b-dd8fd2e9fbec">

<img width="1256" alt="Screenshot 2023-08-13 at 22 38 25" src="https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/ef56f1c4-4725-421a-9b70-15915a96ec6b">

