## 0. Create network

```bash
docker network create "pulsar"
```

```bash
export PULSAR_NETWORK=pulsar
```

List the networks

```bash
docker network ls
```

## 1. Setup zookeeper

```bash
mkdir -p data/zookeeper0
```

```bash
docker run -it --rm -d \
    -v $(pwd)/conf/cluster0:/pulsar/conf \
    -v $(pwd)/data/zookeeper0:/pulsar/data/zookeeper \
    --network ${PULSAR_NETWORK} \
    --name zookeeper0 \
    --hostname zookeeper0 \
    -p 2181:2181 \
    apachepulsar/pulsar-all:2.5.0 \
    bin/pulsar zookeeper
```

### 1.1. Health Check

check the log to see if zookeeper is running

```
docker logs -f zookeeper0
```

health check

```
telnet localhost 2181
```

send `ruok` command to zookeeper and you should see `imok` is returned.

### 2. Initialize Cluster Metadata

Connect to the zookeeper cluster.

```
docker exec -it zookeeper0 bash
```

Initialize the pulsar cluster metadata.

```
export PULSAR_CLUSTER="cluster0"
```

```
bin/pulsar initialize-cluster-metadata \
    --cluster ${PULSAR_CLUSTER} \
    --zookeeper zookeeper0:2181 \
    --configuration-store zookeeper0:2181 \
    --web-service-url http://${PULSAR_CLUSTER}-broker0:8080 \
    --web-service-url-tls https://${PULSAR_CLUSTER}-broker0:8443 \
    --broker-service-url pulsar://${PULSAR_CLUSTER}-broker0:6650 \
    --broker-service-url-tls pulsar+ssl://${PULSAR_CLUSTER}-broker0:6651
```

Connect to zookeeper shell to see what is created on zookeeper.

```
bin/pulsar zookeeper-shell
```

### 3. Start bookies

#### 3.1 Start first bookie

```bash
mkdir -p data/cluster0-bookkeeper0
mkdir -p logs/cluster0-bookkeeper0
```

```bash
docker run -it --rm -d \
    -v $(pwd)/conf/cluster0/bookkeeper0:/pulsar/conf \
    -v $(pwd)/data/cluster0-bookkeeper0:/pulsar/data/bookkeeper \
    -v $(pwd)/logs/cluster0-bookkeeper0:/pulsar/logs \
    --network ${PULSAR_NETWORK} \
    --name cluster0-bookkeeper0 \
    --hostname cluster0-bookkeeper0 \
    -p 3181:3181 \
    -p 8001:8000 \
    apachepulsar/pulsar-all:2.5.0 \
    bin/pulsar bookie
```

On zookeeper node, check if the bookie is registered.

On bookkeeper0 node, do a `simpletest` to verify the bookie is working as expected.

```bash
bin/bookkeeper shell simpletest -e 1 -w 1 -a 1 10
```

#### 3.2 Start second bookie

```bash
mkdir -p data/cluster0-bookkeeper1
mkdir -p logs/cluster0-bookkeeper1
```

```bash
docker run -it --rm -d \
    -v $(pwd)/conf/cluster0/bookkeeper1:/pulsar/conf \
    -v $(pwd)/data/cluster0-bookkeeper1:/pulsar/data/bookkeeper \
    -v $(pwd)/logs/cluster0-bookkeeper1:/pulsar/logs \
    --network ${PULSAR_NETWORK} \
    --name cluster0-bookkeeper1 \
    --hostname cluster0-bookkeeper1 \
    -p 3182:3181 \
    -p 8002:8000 \
    apachepulsar/pulsar-all:2.5.0 \
    bin/pulsar bookie
```

##### Health check

- check docker log
- check http endpoint
    ```bash
    curl localhost:8002/metrics
    ```
    ```bash
    curl localhost:8002/api/v1/config/server_config
    ```

#### 3.3 Start third bookie

```bash
mkdir -p data/cluster0-bookkeeper2
mkdir -p logs/cluster0-bookkeeper2
```

```bash
docker run -it --rm -d \
    -v $(pwd)/conf/cluster0/bookkeeper2:/pulsar/conf \
    -v $(pwd)/data/cluster0-bookkeeper2:/pulsar/data/bookkeeper \
    -v $(pwd)/logs/cluster0-bookkeeper2:/pulsar/logs \
    --network ${PULSAR_NETWORK} \
    --name cluster0-bookkeeper2 \
    --hostname cluster0-bookkeeper2 \
    -p 3183:3181 \
    -p 8003:8000 \
    apachepulsar/pulsar-all:2.5.0 \
    bin/pulsar bookie
```

##### Health check

- check docker log
- check http endpoint
    ```bash
    curl localhost:8003/metrics
    ```
    ```bash
    curl localhost:8003/api/v1/config/server_config
    ```

### 4. BookKeeper Cluster Health Check

After all 3 bookies are up running, you can run the following commands to verify if the cluster is running as expected.

```
bin/bookkeeper shell simpletest
```

### 5. Start brokers


```
docker run -it --rm -d \
    -v $(pwd)/conf/cluster0:/pulsar/conf \
    --network ${PULSAR_NETWORK} \
    --name cluster0-broker0 \
    --hostname cluster0-broker0 \
    -p 6650:6650 \
    -p 8080:8080 \
    apachepulsar/pulsar-all:2.5.0 \
    bin/pulsar broker
```

```
docker run -it --rm -d \
    -v $(pwd)/conf/cluster0:/pulsar/conf \
    --network ${PULSAR_NETWORK} \
    --name cluster0-broker1 \
    --hostname cluster0-broker1 \
    -p 6651:6650 \
    -p 8081:8080 \
    apachepulsar/pulsar-all:2.5.0 \
    bin/pulsar broker
```

### 6. Broker health check

on broker node

```
bin/pulsar-admin tenants list
```

### 7. Start proxies

```
docker run -it --rm -d \
    -v $(pwd)/conf/cluster0:/pulsar/conf \
    --network ${PULSAR_NETWORK} \
    --name cluster0-proxy \
    --hostname cluster0-proxy \
    -p 6750:6650 \
    -p 8180:8080 \
    apachepulsar/pulsar-all:2.5.0 \
    bin/pulsar proxy
```

### 8. Proxy health check

on your laptop, configure your client tool to talk to the proxy, and run:

```
bin/pulsar-admin tenants list
```

### 9. Start prometheus

```
docker run -it --rm -d \
    -p 9090:9090 \
    -v $(pwd)/conf/cluster0/prometheus.yml:/etc/prometheus/prometheus.yml \
    --network ${PULSAR_NETWORK} \
    --name cluster0-prometheus \
    --hostname cluster0-prometheus \
    prom/prometheus
```

### 10. Start grafana

```
docker run -it --rm -d \
    -p 3000:3000 \
    --network ${PULSAR_NETWORK} \
    --name cluster0-grafana \
    --hostname cluster0-grafana \
    -e PULSAR_PROMETHEUS_URL="http://cluster0-prometheus:9090" \
    -e PULSAR_CLUSTER="${PULSAR_CLUSTER}" \
    streamnative/apache-pulsar-grafana-dashboard:latest
```

### 11. Start Pulsar Manager

```bash
mkdir -p data/cluster0-pm
```

```
docker run -it --rm -d \
    -p 9527:9527 \
    -e REDIRECT_HOST=http://localhost \
    -e REDIRECT_PORT=9527 \
    -e DRIVER_CLASS_NAME=org.postgresql.Driver \
    -e URL='jdbc:postgresql://127.0.0.1:5432/pulsar_manager' \
    -e USERNAME=pulsar \
    -e PASSWORD=pulsar \
    -e LOG_LEVEL=DEBUG \
    -v $(pwd)/data/cluster0-pm:/data \
    --network ${PULSAR_NETWORK} \
    --name cluster0-pm \
    --hostname cluster0-pm \
    apachepulsar/pulsar-manager:v0.1.0 /bin/sh
```