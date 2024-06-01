A quickstart of deploying a [zookeeper](https://zookeeper.apache.org/) cluster with [observer](https://zookeeper.apache.org/doc/r3.9.2/zookeeperObservers.html) in TLS using [Docker Compose](https://docs.docker.com/compose/) :whale2:

## IP Plan

| Instance | IP               | Port | HostPort | Role        |
| -------- | ---------------- | ---- | -------- | ----------- |
| zk1      | 192.168.50.11/16 | 2281 | 12281    | Participant |
| zk2      | 192.168.50.12/16 | 2281 | 22281    | Participant |
| zk3      | 192.168.50.13/16 | 2281 | 32281    | Participant |
| zk4      | 192.168.50.14/16 | 2281 | 42281    | Observer    |

## Steps

1. Create network

   ```bash
   $ docker network create --subnet=192.168.0.0/16 local
   ```

2. Docker Compose Up

   ```bash
   $ docker compose up
   ```

3. Verify

   ```bash
   $ docker exec zk1 -it sh
   
   # inside
   zkServer.sh status
   
   export CLIENT_JVMFLAGS="-Dzookeeper.clientCnxnSocket=org.apache.zookeeper.ClientCnxnSocketNetty -Dzookeeper.client.secure=true -Dzookeeper.ssl.keyStore.location=/tls/zk1.jks -Dzookeeper.ssl.keyStore.password=password -Dzookeeper.ssl.trustStore.location=/tls/truststore.jks -Dzookeeper.ssl.trustStore.password=password"
   
   zkCli.sh -server zk1:2281 ls -R /
   ```
   
4. (Optinoal) Uncomment busybox section in `./compose.yaml` for telnet 4LW

5. Enjoy :smile:

6. (Clean up)

   ```bash
   $ docker compose stop && rm -rf ./dataLog/zk*/*
   ```

   

   

