A quickstart of deploying a master/slave redis with sentinel using [Docker Compose](https://docs.docker.com/compose/) :whale2:

## IP Plan

- Sentinel{0..2} → Master{0..2} ← Slave{1..2}
- Credentials of redis-cli (see more in redis.conf)
  - master: `master/masterpwd`
  - slave1: `slave1/slave1pwd`
  - slave2:  `slave2/slave2pwd`

| Instance  | IP               | Port | HostPort |
| --------- | ---------------- | ---- | -------- |
| Master0   | 192.168.10.10/16 | 6379 | 16380    |
| Slave1    | 192.168.10.11/16 | 6379 | 16381    |
| Slave2    | 192.168.10.12/16 | 6379 | 16382    |
| Sentinel0 | 192.168.10.20/16 | 6379 | 26380    |
| Sentinel1 | 192.168.10.21/16 | 6379 | 26381    |
| Sentinel2 | 192.168.10.22/16 | 6379 | 26383    |

## Steps

1. Docker Compose Up

   ```bash
   $ docker compose up
   ```

2. Verify

   ```bash
   # info
   $ docker exec master redis-cli \
   -p 6379 \
   --user master --pass masterpwd \
   info replication
   
   # write to master
   $ docker exec master redis-cli \
   -p 6379 \
   --user master --pass masterpwd \
   set age 18
   
   # read from slave
   $ docker exec slave1 redis-cli \
   -p 6379 \
   --user slave1 --pass slave1pwd \
   get age 
   
   $ docker exec slave2 redis-cli \
   -p 6379 \
   --user slave2 --pass slave2pwd \
   get age 
   ```

   ```bash
   # info
   $ docker exec sentinel0 redis-cli \
   -p 6379 \
   info sentinel
   
   $ docker exec sentinel1 redis-cli \
   -p 6379 \
   info sentinel
   
   $ docker exec sentinel2 redis-cli \
   -p 6379 \
   info sentinel
   ```

3. Enjoy :smile:

4. (Clean up)

   ```bash
   $ docker compose stop && rm -rf ./tmp/*
   ```

   