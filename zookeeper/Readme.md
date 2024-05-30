A quickstart of deploying a [zookeeper](https://zookeeper.apache.org/) cluster with [observer](https://zookeeper.apache.org/doc/r3.9.2/zookeeperObservers.html) using [Docker Compose](https://docs.docker.com/compose/) :whale2:

## IP Plan

| Instance | IP               | Port | HostPort | Role        |
| -------- | ---------------- | ---- | -------- | ----------- |
| zk1      | 192.168.50.11/16 | 2181 | 12181    | Participant |
| zk2      | 192.168.50.12/16 | 2181 | 22181    | Participant |
| zk3      | 192.168.50.13/16 | 2181 | 32181    | Participant |
| zk4      | 192.168.50.14/16 | 2181 | 42181    | Observer    |

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
   $ docker exec zk1 zkServer.sh \
   status
   
   $ docker exec zk2 zkServer.sh \
   status
   
   $ docker exec zk3 zkServer.sh \
   status
   
   $ docker exec zk1 zkCli.sh \
   -server localhost:2181 \
   ls -R /
   
   $ docker exec zk1 zkCli.sh \
   -server localhost:2281 \
   -tls \
   -trustStore /tls/truststore.jks \
   -trustStorePassword password \
   -keyStore /tls/zk1.jks \
   -keyStorePassword password
   ```
   
   ```bash
   $ docker exec zk1 -it sh
   
   # inside
   CLIENT_JVMFLAGS="-Dzookeeper.clientCnxnSocket=org.apache.zookeeper.ClientCnxnSocketNetty -Dzookeeper.client.secure=true -Dzookeeper.ssl.hostnameVerification=false -Dzookeeper.ssl.keyStore.location=/tls/zk1.jks -Dzookeeper.ssl.keyStore.password=password -Dzookeeper.ssl.trustStore.location=/tls/truststore.jks -Dzookeeper.ssl.trustStore.password=password -Dzookeeper.ssl.hostnameVerification=false"
   
   zkCli.sh -server zk1:2281,zk2:2281,zk3:2281,zk4:2281 ls -R /
   ```
   
4. (Optinoal) Uncomment busybox section in `./compose.yaml` for telnet 4LW

5. Enjoy :smile:

6. (Clean up)

   ```bash
   $ docker compose stop && rm -rf ./dataLog/zk*/*
   ```

   

   

