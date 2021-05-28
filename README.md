## Spark-Mini-Project
* [Project Details](#Project-Details)

## Project-Details

Execute docker commands as follows to setup spark hadoop cluster environment locally. this is easier and lighter on resources compared to Hortonworks HDP sandbox setup.

create project folder:
eg: 
```
mkdir /Users/greenbook/Desktop/springboard/Spark-AutoSales-Project/hadoopspark
```


Docker Hadoop spark commands 
--------------------------------------------------------

```
docker network create -d bridge hadoopspark

docker run --name spark-master --hostname sparkmaster --network=hadoopspark -v "/Users/greenbook/Desktop/springboard/Spark-AutoSales-Project/hadoopspark:/opt/hadoopspark" -p 8090:8080 -p 7077:7077 -d bde2020/spark-master:3.0.1-hadoop3.2

docker run --name spark-worker-1 --network=hadoopspark -p 8081:8081 -e "SPARK_MASTER=sparkmaster:7077" -d bde2020/spark-worker:3.0.1-hadoop3.2

docker run --name spark-worker-2 --network=hadoopspark -p 8082:8081 -e "SPARK_MASTER=sparkmaster:7077" -d bde2020/spark-worker:3.0.1-hadoop3.2

docker run --name namenode --network=hadoopspark -v "/Users/greenbook/Desktop/springboard/Spark-AutoSales-Project/hadoopspark:/opt/hadoopspark" -e "CORE_CONF_fs_defaultFS=hdfs://namenode:9000" -e "CLUSTER_NAME=hadooptest" -p 9870:9870 -p 9000:9000 -d bde2020/hadoop-namenode

docker run --name datanode --network=hadoopspark -e "CORE_CONF_fs_defaultFS=hdfs://namenode:9000" -d bde2020/hadoop-datanode

```

verify docker containers are up and running 

![Alt text](/images/container-status.png?raw=true "screenshot")


edit sparkcontext to update with namenode server and port

![Alt text](/images/namenode.png?raw=true "screenshot")

how to submit job:
./spark-submit --master spark://172.23.0.2:7077 /opt/hadoopspark/autoinc_spark.py

![Alt text](/images/running-job.png?raw=true "screenshot")

output file 

![Alt text](/images/output.png?raw=true "screenshot")