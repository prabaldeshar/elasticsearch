# Kafka Setup Procedure
## 1. Download the following file. 
This is the connector that will connect us from Kafka to ElasticSearch.
https://www.confluent.io/hub/confluentinc/kafka-connect-elasticsearch

When it is done, copy the folder to kafka/plugins 

## 2. Go to kafka/etc/kafka/connect-distributed.properties
1. Find the line where we which contains "plugin.path"
2. Add the kafka/plugins path there. 
```
plugin.path = /home/yugant-terakoya/Desktop/kafka_2.13-3.2.1/plugins
```

## 3. follow the following steps to run the servers.
(.bat in server start if windows)
(.sh for linux)

1. Start the zookeeper server. 
bin/zookeeper-server-start etc/kafka/zookeeper.properties 

2. Start the kafka sever
bin/kafka-server-start etc/kafka/server.properties

3. Start the connector server
bin/connect-distributed etc/kafka/connect-distributed.properties

## 4. Go to your terminal and check if the connector is available
curl -s localhost:8083/connector-plugins|jq '.[].class'

We are looking for ElasticSourceConnector.


## 5. Create a connector config.
```
{       
  "name": "elastic-source",
   "config": {
            "connector.class":"com.github.dariobalinzo.ElasticSourceConnector",
            "tasks.max": "1",
            "es.host" : "localhost",
            "es.port" : "9200",
	          "es.scheme" : "http",
	          "poll.interval.ms": "5000",
            "index.prefix" : "books",
            "topic.prefix" : "es_",
            "incrementing.field.name" : "timestamp",
            "behavior.on.null.values": "ignore",
            "behavior.on.malformed.documents": "ignore",
            "drop.invalid.message": "true"
        }
}

```

## 6. Add a new data to ES.

## 7. Try to consume the topic named es_books using the folowing code in the terminal. 
bin/kafka-console-consumer --topic es_books --from-beginning --bootstrap-server localhost:9092

