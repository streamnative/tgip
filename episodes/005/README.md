# Basic information

- Topic: Taking a Deep-Dive into Apache Pulsar Architecture for Performance Tuning

- Host: Sijie Guo [@sijieg](https://twitter.com/sijieg)

- Date: 04/10/2020

- Time: 1PM PST

- Duration: 1hr

- Recorded video link: https://www.youtube.com/watch?v=8O_RNYHX9tI&list=PLqRma1oIkcWhWAhKgImEeRiQi5vMlqTc-&index=2

- [Show Notes](https://hackmd.io/MCQYSFTuThidt-KDCeFtrw)

## Week in Review

- Native Go Client Release 0.1.0
    - https://github.com/apache/pulsar-client-go/tree/v0.1.0
- Pulsar 2.5.1 release candidates are under voting
    - https://lists.apache.org/thread.html/r7b2783d13007e621e2e880a37d808e6a9ec178ece2090e117b8c68e6%40%3Cdev.pulsar.apache.org%3E
- Pulsar Flink Connector
    - Superseded by a newer version of pulsar-flink in Flink Packages.
    - https://flink-packages.org/packages/pulsar-flink-connector
    - https://cwiki.apache.org/confluence/display/FLINK/FLIP-72%3A+Introduce+Pulsar+Connector
    - https://issues.apache.org/jira/browse/FLINK-14146
- New PIPs
    - PIP-61: Advertised multiple addresses
        - https://github.com/apache/pulsar/wiki/PIP-61%3A-Advertised-multiple-addresses
    - PIP-62: Move connectors, adapters and pulsar-presto to separate repositories
        - https://github.com/apache/pulsar/wiki/PIP-62%3A-Move-connectors%2C-adapters-and-Pulsar-Presto-to-separate-repositories

## Show Notes

- Understand the fundamental data structure - event stream / distributedlog
- Understand the Pulsar architecture - clients / brokers / bookies
- Storage: Bookies
    - It is essentially an entry database: <lid, eid> -> entry
    - Journal
    - Ledger Storage
        - Index DB
        - Write cache & Read cache
- Managed Ledger & BookKeeper client
    - Ledger persistence settings
    - Ledger rollover settings
    - Entry cache
    - Read from bookkeeper
- Brokers
    - Understand Namespace Bundle
    - Bundle split & load shedding
        - Split algorithm
        - load shedding
- Clients
    - Producer
        - Pendinig Queue
        - Batching
        - Routing
    - Consumer/Reader
        - Receiver Queue

---

![](https://github.com/streamnative/tgip/blob/master/image/005.png)
