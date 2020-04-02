# Basic information

- Topic: Secure a Pulsar cluster with TLS

- Host: Sijie Guo [@sijieg](https://twitter.com/sijieg)

- Date: 03/27/2020

- Time: 1PM PST

- Duration: 1hr

- Recorded video link: https://www.youtube.com/watch?v=aP31A-ntHLA&list=PLqRma1oIkcWhWAhKgImEeRiQi5vMlqTc-&index=1

- [Show Notes](https://hackmd.io/-AdXA_FUT9i3gXRp5qLLLw?view)

## Week in Review

- Kafka-on-Pulsar open sourced!
    - https://github.com/streamnative/kop
    - https://streamnative.io/blog/tech/2020-03-24-bring-native-kafka-protocol-support-to-apache-pulsar/
- Kafka-on-Pulsar Webinar on 10 AM PST, March 31
    - Co-hosted by StreamNative and OVHcloud
    - Registration Link: https://zoom.us/webinar/register/6515842602644/WN_l_i-3ekDSg6PwPFn7tqRvA

## Show Notes

- TLS Enabled Server and Client
    - A TLS-enabled server contains a private key and a public certificate:
        - The private key never leaves the server.
        - The public certificate is exposed through TLS.
    - TLS-enabled clients verify that they trust the server by validating the public certificate against their own set of trusted certificates.
- TLS Encryption
    - Certificate authority (CA)
        - ca.cert.pem: public certificate
        - ca.key.pem: private ca key, used for signing other certificates
        - A well-known Certificate Authority (CA) can generate certificates
        - A certificate locally without external approval (self-signed certificate)
    - Server certificate
        - create certificate requests and sign them with the CA
        - broker.cert.pem: server certificate
        - broker.key.pem: key
    - Client
        - ca.cert.pem
- Keystore & Truststore
    - Truststore and keystore contents differ depending on whether they are used for clients or servers:
        - For servers: the truststore contains certificates of the trusted clients, the keystore contains the private and public key of the server.
        - For clients: the truststore contains certificates of the trusted servers, the keystore contains the private and public key of the client.
- Secure every connection between components
    - client
    - broker
    - proxy
    - function worker
    - bookie
    - zookeeper
    - pulsar manager
- Tool
    - `pulsarctl security-tool`
    - kubernetes deployment
        - cert-manager
            - self-signing
            - let's encrypt

## Reference Links

- Pulsar TLS: http://pulsar.apache.org/docs/en/security-tls-transport/
- BookKeeper TLS: https://bookkeeper.apache.org/docs/latest/security/tls/
- ZooKeeper TLS: https://cwiki.apache.org/confluence/display/ZOOKEEPER/ZooKeeper+SSL+User+Guide
- Pulsar Manager TLS: https://streamnative.io/docs/v1.0.0/configure/control-center/pulsar-manager/#tls-encryption

---

![](https://github.com/streamnative/tgip/blob/master/image/003.png)
