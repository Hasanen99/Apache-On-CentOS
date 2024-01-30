# Apache-On-CentOS

**Note**: 

1- We have started with HADOOP but we'll leave it for later now.

2- We'll deploy everything on Docker.

## CentOS
Pull the centos 7 docker container, and run `yum update -y` 

Install `net-tools` to use "ifconfig" command to check the current IP of the container.

## Install and activate Kafka
1- From https://kafka.apache.org/ and download the last updated version .tgz file, by using `wget` package.

2- After so, uncompress the file using `tar` command, and then you can rename the new folder using `mv` if you want.

**Notes:** Inside the new folder you have two main directories you will use, The first one is (bin) contains operations executable shell files, and the second one is (config) which contains the default configurations file and more we may add in the future.

3- Kafka needs zookeeper to run, so the first thing we should do is to go to bin directory and run: 

`./zookeeper-server-start.sh -daemon ../config/zookeeper.properties` 

`-daemon` makes the zookeeper run in the background and releases the terminal to use it (try to run without -daemon).

4- To check if zookeeper runs correctly use `telnet localhost 2181` and the use `stat` commands to see zookeeper status -you may face the problem of zookeeper whitelist like https://stackoverflow.com/questions/62667788/how-do-i-initialize-the-whitelist-for-apache-zookeeper - also you can simply use zookeeper-shell.sh.

5- Now let's run the Kafka server, by a bit same method, `./kafka-server-start.sh ../config/server.properties` and now congrats!! WE HAVE LIFT OFF! -If not :( no worry .. keep reading-

-------------------------- Configure the server.properties file --------------------------

In `################## Socket Server Settings ###################` section we need to set-up the listeners (https://www.confluent.io/blog/kafka-listeners-explained/)

Such as: 
listeners=LISNERONE://localhost:9092

advertised.listeners=LISNERONE://localhost:9092

listener.security.protocol.map=LISNERONE:PLAINTEXT,SSL:SSL,SASL_PLAINTEXT:SASL_PLAINTEXT,SASL_SSL:SASL_SSL

inter.broker.listener.name=LISNERONE

------------------------------- Kafka Structure --------------------------------------------

A cluster is a set of nodes (Not Precise), Kafka deals with parts inside nodes, and those parts are Kafka brokers, we set up each broker by the process mentioned above with the server.properties configuration file. 

Each time we create a topic, we need to specify `PartitionCount` which describes how many parts will this broker be, and `replication factor` which sets how many copy will be for each partition, this number should be no more than the count of brokers.

Let us say, we have a cluster of two nodes, node1 has 2 brokers (0,1) and node2 has 3 brokers (2,3,4) when creating a topic with 4 partitions, replication factor of 3:

Partition 1: Leader on Broker 1 and 2 other copies on Broker 3 & Broker 4

Partition 2: Leader on Broker 2 and 2 other copies on Broker 1 & Broker 5

Partition 1: Leader on Broker 3 and 2 other copies on Broker 1 & Broker 2

Partition 1: Leader on Broker 5 and 2 other copies on Broker 4 & Broker 3


