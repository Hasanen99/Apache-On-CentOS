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

3- Kafka need zookeeper to run, so the first thing we should do is to go to bin directory and run: 

`./zookeeper-server-start.sh -daemon ../config/zookeeper.properties` 

`-daemon` makes the zookeeper run in the background and releases the terminal to use it (try to run without -daemon).

4- To check if zookeeper runs correctly use `telnet localhost 2181` and the use `stat` command to see zookeeper status -you may face the problem of zookeeper whitelist like https://stackoverflow.com/questions/62667788/how-do-i-initialize-the-whitelist-for-apache-zookeeper - also you can simply use zookeeper-shell.sh.

5- Now let's run the Kafka server, by a bit same method, `./kafka-server-start.sh ../config/server.properties` and now congrats!! WE HAVE LIFT OFF! -If not :( no worry .. keep reading-

-------------------------- Configure the server.properties file --------------------------

