# KStream to KTable Pipe Example

Create the log compacted topic that backs the KTable. Send some data to the topic as key:value pairs.

```bash
kafka_2.12-2.3.1 nico$ bin/kafka-topics.sh --zookeeper localhost:2181 --create --topic my.compacted.topic --replication-factor 1 --partitions 1 --config min.insync.replicas=1 --config  cleanup.policy=compact --config segment.bytes=1048576
kafka_2.12-2.3.1 nico$ bin/kafka-topics.sh --list --bootstrap-server localhost:9092
kafka_2.12-2.3.1 nico$ bin/kafka-console-producer.sh --broker-list localhost:9092 --topic my.compacted.topic --property "parse.key=true" --property "key.separator=:"
>foo:bar
>foo:zoo
>pie:cake
>key2:value
```

Run the app, it will print out updates for given key.

```bash
mvn clean package
mvn exec:java -Dexec.mainClass=myapps.Pipe
```

Below is how you'll check the topic, not compaction happens on a per segment basis, with a 1MB segment size it will be a while
before compaction takes place.

```bash
kafka_2.12-2.3.1 nico$ bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic my.compacted.topic
```

All the state-stores are stored in the location specified in state.dir. If not specified, it is /tmp/kafka-streams/<app-id> directory.

```bash
$ ls /tmp/kafka-streams/streams-pipe/0_0/rocksdb/KSTREAM-REDUCE-STATE-STORE-0000000001/
000006.log                      CURRENT                         LOG                             OPTIONS-000008
000011.sst                      IDENTITY                        LOG.old.1572837629313147        OPTIONS-000010
000012.sst                      LOCK                            MANIFEST-000005
```

## References

### Tutorial: Write a Kafka Streams Application

[Tutorial: Write a Kafka Streams Application](https://kafka.apache.org/23/documentation/streams/tutorial)

#### How I Created From Scratch

```bash
mvn archetype:generate -DarchetypeGroupId=org.apache.kafka -DarchetypeArtifactId=streams-quickstart-java -DarchetypeVersion=2.3.1 -DgroupId=streams.examples -DartifactId=streams.examples -Dversion=0.0.1-SNAPSHOT -Dpackage=myapps
```

### KStream to KTable

[Apparently common requirement](https://stackoverflow.com/questions/42937057/kafka-streams-api-kstream-to-ktable).
