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

## Procedures
Install Apache Kafka on EC2 by installing an EC2 instance and install Zookeeper, KafkaConsumer and KafkaProducer on the ec2 instance on AWS. Kindly ensure the instance-type has enough resources for the Apache Kafka. You will need to open 3 differenet VM tabs for the dependencies.

<img width="943" alt="Screenshot 2023-09-17 at 21 24 48" src="https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/bc437fc4-2468-442a-9f91-e37d674601fb">


Create an S3 bucket for data ingestion.

<img width="796" alt="Screenshot 2023-09-17 at 21 28 00" src="https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/5c1c1250-d2b1-4625-a9e3-87f581b7698a">


Configure a lambda function, use runtime as python and add the python code on the configuration tab and deploy.


Create AWS Glue and crawler for the file and run the query on AWS Athena


<img width="1252" alt="Screenshot 2023-08-13 at 22 25 55" src="https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/ed5f8c42-d619-49fd-a7cd-af766098e6c2">

<img width="1275" alt="Screenshot 2023-08-13 at 22 26 17" src="https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/196d3665-3237-439d-9714-876c6b3d6ff3">

<img width="1253" alt="Screenshot 2023-08-13 at 22 38 19" src="https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/f3c67fbb-81d3-43f0-906b-dd8fd2e9fbec">

<img width="1256" alt="Screenshot 2023-08-13 at 22 38 25" src="https://github.com/Mamiololo01/Real_time-data_pipeline-with-Kafka/assets/67044030/ef56f1c4-4725-421a-9b70-15915a96ec6b">

