Elasticsearch Integration ![Build Status](https://travis-ci.org/kamon-io/kamon-elasticsearch.svg?branch=master)
==========================

[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/kamon-io/Kamon?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/io.kamon/kamon-elasticsearch_2.11/badge.svg)](https://maven-badges.herokuapp.com/maven-central/io.kamon/kamon-elasticsearch_2.11)


The `kamon-elasticsearch` module brings bytecode instrumentation to trace elasticsearch requests

<p class="alert alert-info">
The <b>kamon-elasticsearch</b> module requires you to start your application using the AspectJ Weaver Agent. Kamon will warn you
at startup if you failed to do so.
</p>

The bytecode instrumentation provided by the `kamon-elasticsearch` module hooks into Elasticsearch's internals to automatically
start and finish segments for requests that are issued within a trace. This translates into you having metrics about how
the requests you are doing are behaving.

### Getting Started

Kamon Elasticsearch module is currently available for Scala 2.10, 2.11 and 2.12.

Supported releases and dependencies are shown below.

| kamon-elasticseach  | status | jdk  | scala            | akka   |
|:------:|:------:|:----:|------------------|:------:|
|  0.6.5 | stable | 1.7+, 1.8+ | 2.10, 2.11, 2.12  | 2.3.x, 2.4.x |

To get started with SBT, simply add the following to your `build.sbt`
file:

```scala
libraryDependencies += "io.kamon" %% "kamon-elasticsearch" % "0.6.5"
```
****
### Metrics ###

The following metrics will be recorded:

* __reads__: a histogram that tracks the reads requests latency (Get and Search).
* __writes__: a histogram that tracks the writes requests latency (Index, Update, and Delete).
* __slows__: a simple counter with the number of measured slow requests.
* __errors__: a simple counter with the number of failures.

### Naming Segments ###

By default, the name generator bundled with the `kamon-elasticsearch` module will use the `org.elasticsearch.action.ActionRequest` class name
to assign a name to the automatically generated segment (i.e GetRequest, SearchRequest, IndexRequest, etc). Currently, the only way to override
that name would be to provide your own implementation of `kamon.elasticsearch.ElasticsearchNameGenerator` which is used to assign the segment name

### Slow Requests ###

Requests that take longer to execute than the configured `kamon.elasticsearch.slow-query-threshold` can be processed by user-defined
`kamon.elasticsearch.SlowRequestProcessor`. The default processor logs a warning message

### Error Processor ###
Requests that error can be processed by user-defined `kamon.elasticsearch.ElasticsearchErrorProcessor`. The default processor logs an error message

### Configuration ###

```typesafeconfig 
kamon {
  elasticsearch {
    slow-query-threshold = 2 seconds

    # Fully qualified name of the implementation of kamon.elasticsearch.SlowRequestProcessor.
    slow-query-processor = kamon.elasticsearch.DefaultSlowRequestProcessor

    # Fully qualified name of the implementation of kamon.elasticsearch.ElasticsearchErrorProcessor.
    elasticsearch-error-processor = kamon.elasticsearch.DefaultElasticsearchErrorProcessor

    # Fully qualified name of the implementation of kamon.elasticsearch.ElasticsearchNameGenerator that will be used for assigning names to segments.
    name-generator = kamon.elasticsearch.DefaultElasticsearchNameGenerator
  }
}
```
