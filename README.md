# Search Cluster
The search cluster consists of the standard components RabbitMQ and ElasticSearch components, with a Indexer and 
SearchQuery components to index and query the data.

![Components](document/SearchArchitecture.png)

## Indexer
The Index consumes the messages off the `indexDocument` queue and persists them into an Elastic index (index name based on the 
date of the message - if parse-able).

## SearchQuery
A `golang` application that exposes a RestAPI that queries the ElasticSearch indices bash on the parameters.

## RabbitMQ
Standard RabbitMQ server, with an initialisation script that once the server has start creates the Publisher and Receiver 
users using the password secrets.

## ElasticSearch
This is a standard version of a single node ElasticSearch, without the X-pack.

## ElasticSearch Template
This image contains the latest index template pattern; the contain will apply the current template to 
the ElasticSearch instance on the node `elasticSearch`. Once applied this sidecar image then shuts down.

# Installation
Ensure you run the following before you try and start the swarm
```
sudo sysctl -w vm.max_map_count=262144
```
Once running you can access Elastic via the localhost:9200 url
this will need to be changed going forward and put behind a some type of
authentication.

## Secrets
Do this to run you need to create the following secrets that will be used to define access to the rabbitmq.

```
echo "<password>" | docker secret create indexingReceiverPassword -
echo "<password>" | docker secret create indexingPublisherPassword -
echo "<password>" | docker secret create rabbitSecretsPassword -
```
To do this interactively you can use the following
```
read -p "create receiverMqPassword : " pwd ; echo $pwd | docker secret create indexingReceiverPassword - 
read -p "create publishedMqPassword : " pwd ; echo $pwd | docker secret create indexingPublisherPassword -
read -p "create adminMqPassword : " pwd ; echo $pwd | docker secret create rabbitSecretsPassword -

```

To start the cluster  
```
docker stack deploy --force -c docker-compose.yaml search
```

You may want to pull the latest image down.

```
docker pull guidof/documentindexer-elasticsearch:latest 
docker pull guidof/documentindexer-indextemplates:latest
docker pull guidof/documentindexer-rabbitmq:latest 
docker pull guidof/documentindexer-searchquery:latest
```


//TODO Create Kafka version
