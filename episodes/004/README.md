# Basic information

- Topic: Deep dive into authentication and authorization

- Host: Sijie Guo [@sijieg](https://twitter.com/sijieg)

- Date: 04/03/2020

- Time: 1PM PST

- Duration: 1hr

- Recorded video link: https://www.youtube.com/watch?v=sTISVpyq73o&list=PLqRma1oIkcWhWAhKgImEeRiQi5vMlqTc-&index=4&t=99s

- [Show Notes](https://hackmd.io/fSbRTGhBSV2S_BZUywShEA)

## Week in Review

- Kafka-on-Pulsar on-demand webinar!
    - https://www.youtube.com/watch?v=gL6hzRtij8M
- Taking a Deep-Dive into Apache Pulsar Architecture for Performance Tuning:
    - https://streamnative.io/whitepaper/taking-a-deep-dive-into-apache-pulsar-architecture-for-performance-tuning/

## Show Notes

- Understand Role
- Authentication: who are you?
- Authorization
    - superusers
    - tenant-admin
    - user roles: produce/consume/functions
- Enable Authentication / Authorization
    - Component by Component
- Role
    - Single Step:
        - mTLS: common name
        - JWT: token subject
            - PIP-55: https://github.com/apache/pulsar/wiki/PIP-55:-Refresh-Authentication-Credentials
    - Multiple Steps:
        - https://github.com/apache/pulsar/wiki/PIP-30%3A-change-authentication-provider-API-to-support-mutual-authentication
        - Kerberos: http://pulsar.apache.org/docs/en/security-kerberos/
- Flink Connector
    - https://github.com/streamnative/pulsar-flink#authentication-configurations
- KoP
    - Multi Tenancy

---

![](https://github.com/streamnative/tgip/blob/master/image/004.jpg)
