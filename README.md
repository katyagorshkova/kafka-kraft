# How to create docker compose for running Kafka in Kraft mode

Before starting the clusterId must be generated and the storage must be formatted with a special tool.
The command for generating uuid:

`kafka-storage.sh random-uuid`

The command for formatting:

`kafka-storage.sh format`

The docker compose has 4 containers:

- **kafka-gen** - if the file clusterID/clusterID does not exist this script generates a new file with a cluser id
- **kafka1(2,3)** - kafka containers, each of them serves as broker and controller. These containers wait for the clusterID file, format the storage directory and then start. 

After starting kafka cluster we can create topic:

`docker exec -ti kafka1 /usr/bin/kafka-topics --create  --bootstrap-server kafka1:19092,kafka2:19093,kafka3:19094 --replication-factor 2 --partitions 4 --topic topic1`

Send the data:

`docker exec -ti kafka1 /usr/bin/kafka-console-producer --bootstrap-server kafka1:19092,kafka2:19093,kafka3:19094 --topic topic1`

Read the data:

`docker exec -ti kafka1 /usr/bin/kafka-console-consumer --bootstrap-server kafka1:19092,kafka2:19093,kafka3:19094 --topic topic1 --from-beginning`
