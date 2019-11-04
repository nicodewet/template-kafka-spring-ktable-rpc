# template-kafka-spring-ktable-rpc

We want to have an app that has a KTable based RESTful API that is served by a terminal KTable (RocksDB). In production a log compacted topic will serve the KTable (for recovery) which will itself be run from an in-memory filesystem (since
we can recover using Kafka).

The plan:

* [Part A] Get as far as we can without Spring, just Kafka, Kafka Streams and a Java class
* [Part B] Layer Spring Boot ontop, we want a simple HTTP GET endpoint to fetch by ID and a InteractiveQueryService
* [Part C] Layer Kubernetes ontop

## Part A - KStream to KTable Pipe Example

See the [streams.examples](/streams.examples) directory for this work - it represents out foundation without Spring.

At the end of this, you may be frustated since you'll know you are writing to the KTable, but its hard to verify and
not that useful.

Ontop the next step!

## Part B - Spring Boot RESTful Layer

WIP.

## Part C - Deploy using Kubernetes

WIP.

### References

* [Spring for Apache Kafka Deep Dive Part 2](https://www.confluent.io/blog/spring-for-apache-kafka-deep-dive-part-2-apache-kafka-spring-cloud-stream)
* [itzg/KafkaStreamsConfig.java](https://gist.github.com/itzg/e3ebfd7aec220bf0522e23a65b1296c8)