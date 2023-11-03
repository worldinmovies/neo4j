# Neo4J Service


http://localhost:7474/browser/

```bash
jenv local corretto64-17.0.6

mvn -s settings.xml clean install

java -jar -Dspring.profiles.active=local target/neo4j-service-0.0.1-SNAPSHOT.jar

docker exec -ti worldinmovies_kafka_1 /opt/bitnami/kafka/bin/kafka-topics.sh --delete --topic movie --bootstrap-server localhost:9092
```


# To backup and restore
```bash
docker compose stop neo4j

docker run -ti --detach --rm \
  --volume=$PWD/persistent-data/neo4j-data:/data/ \
  neo4j:5 \
  neo4j-admin database dump neo4j --to-path=/data/

cp persistent-data/neo4j-data/neo4j.dump .

docker compose down neo4j

rm -rf persistent-data/neo4j-data/*

# Restore
cp neo4j.dump persistent-data/neo4j-data/

docker run -ti --rm \
  --volume=$PWD/persistent-data/neo4j-data:/data/ \
  neo4j:5 \
  neo4j-admin database load neo4j --from-path=/data/
  
docker compose up -d neo4j


```
