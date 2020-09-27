# SparkStreaming
Spark Streaming project using Scala, Spark streaming, integrating with Hive and Kafka.
[Video Demo](later)
## Part 1: Get Twitter popular hashtags
- Need a Twitter credentials fill in twitter.txt
- Need creating an empty folder "checkpoint"
## Part 2: Integrating with Hive
- Run the jar file inside your Cloudera VM (You can use SBT assembly to package the fat jar)

`spark-submit --class "PopularHashtags" "/your/path/PopularTags-assembly-0.1.jar" "/your/path/contains/checkpoint/folder" /user/cloudera/output`
- Create a table in Hive

`CREATE EXTERNAL TABLE popular_hashtags (hashtag STRING, count INT)
STORED AS PARQUET
LOCATION '/user/cloudera/output/data.parquet';`
- Run the query

`SELECT * FROM popular_hashtags
ORDER BY count DESC;`
## Part 3: Run streaming with Kafka
- Install Kafka on your local
  - [Kafka setup instructions for Windows](https://dzone.com/articles/running-apache-kafka-on-windows-os)
  - [Kafka setup instructions for MacOS/Linux](https://medium.com/@Ankitthakur/apache-kafka-installation-on-mac-using-homebrew-a367cdefd273)
- Start Zookeeper

`zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties`
- Start Kafka server

`kafka-server-start /usr/local/etc/kafka/server.properties`
- Register a topic

`kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic testLogs`
- Run the app **first** to begin listening from Kafka and then run the producer

`kafka-console-producer --broker-list localhost:9092 --topic testLogs < your/path/access_log.txt`
