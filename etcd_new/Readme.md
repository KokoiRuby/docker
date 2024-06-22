A quickstart of deploying a [etcd](https://etcd.io/) cluster with [gRPC proxy](https://etcd.io/docs/v3.5/op-guide/grpc_proxy/) using [Docker Compose](https://docs.docker.com/compose/) :whale2:

## IP Plan

| Instance | IP               | Port           | HostPort         |
| -------- | ---------------- | -------------- | ---------------- |
| etcd0    | 192.168.30.10/16 | 2379<br />2380 | 12379<br />12380 |
| etcd1    | 192.168.30.11/16 | 2379<br />2380 | 22379<br />22380 |
| etcd2    | 192.168.30.12/16 | 2379<br />2380 | 32379<br />32380 |
| proxy    | 192.168.30.13/16 | 2379           | 42379            |

## Steps

1. Create network

   ```bash
   $ docker network create --subnet=192.168.0.0/16 local
   ```

2. Docker Compose Up

   ```bash
   $ docker compose up -d
   ```

3. Verify

   ```bash
   $ docker exec proxy etcdctl \
       --endpoints http://etcd0:2379,http://etcd1:2379,http://etcd2:2379 \
       member list
   ```
   
4. (Clean up)

   ```bash
   $ docker compose down
   ```
   

## TLS

1. Create network

   ```bash
   $ docker network create --subnet=192.168.0.0/16 local
   ```

2. Install [cfssl](https://github.com/cloudflare/cfssl)

3. Create keypair

   ```bash
   $ cd tls/ && make all
   ```

4. Docker Compose up

   ```bash
   $ docker compose -f compose-tls.yaml up
   ```

5. Verify

   ```bash
   $ docker exec proxy etcdctl \
       --endpoints https://etcd0:2379,https://etcd1:2379,https://etcd2:2379 \
       --cert /opt/bitnami/etcd/conf/certs/proxy.pem \
       --key /opt/bitnami/etcd/conf/certs/proxy-key.pem \
       --cacert /opt/bitnami/etcd/conf/certs/ca.pem \
       member list
   ```

6. (Clean up)

   ```bash
   $ docker compose -f compose-tls.yaml down
   ```

   