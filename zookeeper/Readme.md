A quickstart of deploying a [zookeeper](https://zookeeper.apache.org/) cluster using [Docker Compose](https://docs.docker.com/compose/) :whale2:

## IP Plan

| Instance | IP               | Port | HostPort |
| -------- | ---------------- | ---- | -------- |
| zk1      | 192.168.50.11/16 | 2181 | 12181    |
| zk2      | 192.168.50.12/16 | 2181 | 22181    |
| zk3      | 192.168.50.13/16 | 2181 | 32181    |
| zk4      | 192.168.50.14/16 | 2181 | 42181    |

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
   ```
   
4. (Optinoal) Uncomment busybox section in ./compose.yaml for telnet 4LW

5. Enjoy :smile:

6. (Clean up)

   ```bash
   $ docker compose stop
   ```

   

   

