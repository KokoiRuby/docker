A quickstart of deploying a [kafka](https://kafka.apache.org/) cluster in [Kraft](https://developer.confluent.io/learn/kraft/) mode using [Docker Compose](https://docs.docker.com/compose/) :whale2:

## IP Plan

| Instance | IP               | Port | HostPort |
| -------- | ---------------- | ---- | -------- |
| kf1      | 192.168.50.21/16 | 9093 | 19093    |
| kf2      | 192.168.50.22/16 | 9093 | 29093    |
| kf3      | 192.168.50.23/16 | 2181 | 39093    |

## Steps

1. Create network

   ```bash
   $ docker network create --subnet=192.168.0.0/16 local
   ```

2. Create data folder

   ```bash
   $ mkdir -p ./data/kf{1..3}
   ```

3. Docker Compose Up

   ```bash
   $ docker compose up
   ```

4. Verify

   ```bash
   $ docker exec kf1 kafka-configs.sh \
    --bootstrap-server kf1:9093,kf2:9093,kf3:9093 \
    --entity-type brokers \
    --describe
   ```

5. Enjoy :smile:

6. (Clean up)

   ```bash
   $ docker compose stop && rm -rf data/kf*/* && rm -rf data/kf*/.*
   ```

