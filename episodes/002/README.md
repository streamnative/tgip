# Basic information

- Topic: Kubernetes Deployment

- Host: Sijie Guo [@sijieg](https://twitter.com/sijieg)

- Date: 03/20/2020

- Time: 1PM PST

- Duration: 1hr

- Recorded video link: https://www.youtube.com/watch?v=wGgEx1M17O0&list=PLqRma1oIkcWhWAhKgImEeRiQi5vMlqTc-&index=4&t=0s

- [Show Notes](https://hackmd.io/4BrPFwTTSvuTh5DS_Jlghg?view)

## Week in Review

- 2020 Pulsar User Survey Report
    - http://pulsar.apache.org/blog/2020/03/17/announcing-the-apache-pulsar-2020-user-survey-report/
- Kafka-on-Pulsar Webinar on 10 AM PST, March 31
    - Co-hosted by StreamNative and OVHcloud
    - Registration Link: https://zoom.us/webinar/register/6515842602644/WN_l_i-3ekDSg6PwPFn7tqRvA

## Show Notes

- Kubernetes Deployment Checklist
    - [ ] How do your applications access your Pulsar cluster?
    - [ ] Where to store the data for ZooKeeper and BookKeeper?
    - [ ] How do you manage and monitor your Pulsar cluster?
- Network
    - Broker vs Proxy
    - Understand Topic Lookup / Service Discovery
    - How do Proxy discover brokers?
- BookKeeper
    - Cookie & Bookie Identifier
        - Bookie Identifier
            - IP
            - Hostname
            - Advertised Address
            - Multiple Listeners (*)
        - DaemonSet vs StatefulSet
    - Persistent Volume vs Local Persistent Volume
    - AutoRecovery
        - running as part of bookies: using `bin/bookkeeper shell`
        - running outside of bookies: `kubectl scale`
- Monitoring
    - Prometheus
    - Grafana
    - Pulsar Manager
        - What is Environment?

## Reference Links

- Local Persistent Volume (Kubernetes 1.14+): https://kubernetes.io/blog/2019/04/04/kubernetes-1.14-local-persistent-volumes-ga/
- StatefulSet: https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
- DaemonSet: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

---

![](https://github.com/streamnative/tgip/blob/master/image/002.png)

