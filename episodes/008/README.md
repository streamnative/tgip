# Basic information

- Topic: What is new in Pulsar 2.5.1?

- Host: Sijie Guo [@sijieg](https://twitter.com/sijieg)

- Date: 05/01/2020

- Time: 1PM PST

- Duration: 1hr

- Recorded video link: https://www.youtube.com/watch?v=qb1WhZSeMzg

- [Show Notes](https://hackmd.io/ripCTdwJSzeoBQad3_3fIA)

## What is New in Pulsar 2.5.1?

- Dependencies updates
    - Netty: 4.1.43 -> 4.1.45.Final
    - Avro: 1.8.x -> 1.9.1
        - 1.9.1: Joda-Time -> JSR-310
        ```bash
        AvroSchema.of(SchemaDefinition.builder()
            .withJSR310ConversionEnabled(true)
            .build())
        ```
    - ZooKeeper: -> 3.5.7
        - Fix split brain on log disk full
            - https://issues.apache.org/jira/browse/ZOOKEEPER-3701
        
- Refresh Authentication Credentials
    - https://github.com/apache/pulsar/wiki/PIP-55%3A-Refresh-Authentication-Credentials

- New bundle split algorithm:
    - range_equally_divide vs topic_count_equally_divide
    ```bash
    bin/pulsar-admin namespaces split-bundle -b 0x00000000_0xffffffff --split-algorithm-name topic_count_equally_divide public/default
    ```
    
- Admin tool:
    - Unload
        - a non-partitioned topic
        - a partition of a partitioned topic
        - *a partitioned topic*

- Namespace Policies
    - Enable/Disable delayed delivery for messages
    - Offloader policies

- Disable subscription auto-creation

- "Inactive" topic deletion
    - `brokerDeleteInactiveTopicsMode`
        - delete_when_no_subscriptions
        - delete_when_subscriptions_caught_up

- Stats & Monitoring
    - Add backlogSize in topicStats
    - lastConsumedTimestamp and lastAckedTimestamp to consumer stats

- Performance
    - Key_Shared dispatching performance

- Stabilitiy
    - OOM
        - maxMessagePublishBufferSizeInMB

- Security
    - BouncyCastle Provider
        - User can choose BC provider and BC-FIPS provider
    - Allow tenant admin to manage subscription permissions

- Clients
    - Python support 3.8
    - CPP Client (remove SSL statical linking)
        - libpulsarnossl.so: libpulsar.so without SSL statically linked
        - libpulsarwithdeps.a


- Deployment
    - Helm: https://github.com/apache/pulsar-helm-chart
    - http://pulsar.apache.org/docs/en/helm-overview/

- Bug fixes
    - Backlog messages that has not yet expired could be deleted before TTL
        - https://github.com/apache/pulsar/pull/6211
    - GetLastMessageId & hasMessageAvailable()
        - https://github.com/apache/pulsar/pull/6511
        - https://github.com/apache/pulsar/pull/6362
        - https://github.com/apache/pulsar/pull/6562
    - Seek
        - Seek to the first message >= timestamp
            - https://github.com/apache/pulsar/pull/6393
    - Topic Compaction
        - Fails for an empty topic
        - Never ends if the value of the last message is an empty batch
        - Fails for a topic with batch messages
            - https://github.com/apache/pulsar/pull/6237

---

![](https://github.com/streamnative/tgip/blob/master/image/008.png)

