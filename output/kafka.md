# Kafka

Kafka output plugin allows to ingest your records into an [Apache Kafka](https://kafka.apache.org/) service. This plugin use the official [librdkafka C library](https://github.com/edenhill/librdkafka) \(built-in dependency\)

## Configuration Parameters

| Key | Description | default |
| :--- | :--- | :--- |
| Format | Specify data format, options available: json, msgpack. | json |
| Message\_Key | Optional key to store the message |  |
| Timestamp\_Key | Set the key to store the record timestamp | @timestamp |
| Brokers | Single of multiple list of Kafka Brokers, e.g: 192.168.1.3:9092, 192.168.1.4:9092. |  |
| Topics | Single entry or list of topics separated by comma \(,\) that Fluent Bit will use to send messages to Kafka. If only one topic is set, that one will be used for all records. Instead if multiple topics exists, the one set in the record by Topic\_Key will be used. | fluent-bit |
| Topic\_Key | If multiple Topics exists, the value of Topic_Key in the record will indicate the topic to use. E.g: if Topic\_Key is \_router_ and the record is {"key1": 123, "router": "route_2"}, Fluent Bit will use topic \_route\_2_. Note that the topic **must** be registered in the Topics list. |  |
| rdkafka.{property} | `{property}` can be any [librdkafka properties](https://github.com/edenhill/librdkafka/blob/master/CONFIGURATION.md) |  |

> Setting `rdkafka.log.connection.close` to `false` and `rdkafka.request.required.acks` to 1 are examples of recommended settings of librdfkafka properties.

## Getting Started

In order to insert records into Apache Kafka, you can run the plugin from the command line or through the configuration file:

### Command Line

The **splunk** plugin, can read the parameters from the command line in two ways, through the **-p** argument \(property\), e.g:

```text
$ fluent-bit -i cpu -o kafka -p brokers=192.168.1.3:9092 -p topics=test
```

### Configuration File

In your main configuration file append the following _Input_ & _Output_ sections:

```text
[INPUT]
    Name  cpu

[OUTPUT]
    Name        kafka
    Match       *
    Brokers     192.168.1.3:9092
    Topics      test
```

