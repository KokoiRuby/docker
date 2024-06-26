A quickstart of deploying a [redis](https://redis.io/) cluster using [Docker Compose](https://docs.docker.com/compose/) :whale2:

## IP Plan

- Sentinel{0..2} → Master{0..2} ← Slave{0..2}
- Credentials of redis-cli: `cluster/clusterpwd`, see more in redis.conf

| Instance  | IP               | Port | HostPort |
| --------- | ---------------- | ---- | -------- |
| Master0   | 192.168.20.10/16 | 6379 | 16380    |
| Master1   | 192.168.20.11/16 | 6379 | 16381    |
| Master2   | 192.168.20.12/16 | 6379 | 16382    |
| Slave0    | 192.168.20.20/16 | 6379 | 26380    |
| Slave1    | 192.168.20.21/16 | 6379 | 26381    |
| Slave2    | 192.168.20.22/16 | 6379 | 26382    |
| Sentinel0 | 192.168.20.30/16 | 6379 | 36380    |
| Sentinel1 | 192.168.20.31/16 | 6379 | 36381    |
| Sentinel2 | 192.168.20.32/16 | 6379 | 36382    |

## Steps

1. Create network

   ```bash
   $ docker network create --subnet=192.168.0.0/16 local
   ```

2. Docker Compose Up

   ```bash
   $ docker compose up
   ```

3. Cluster Meet

   ```bash
   $ while IFS="" read -r ip
   do
   	docker exec cluster-master0 redis-cli \
   	-p 6379 \
   	--user cluster --pass clusterpwd \
   	cluster meet $ip 6379
   	
   done < meetlist
   
   # Verify
   # cluster_known_nodes shall be 6
   $ docker exec cluster-master0 redis-cli \
   -p 6379 \
   --user cluster --pass clusterpwd \
   cluster info
   ```

4. Add Slots

   ```bash
   $ paste masterlist slotlist | while read -r master slot
   do
   	docker exec $master redis-cli \
   	-p 6379 \
   	--user cluster --pass clusterpwd \
   	cluster addslots {$slot}
   
   done
   
   # Verify
   # cluster_state shall be ok
   $ docker exec cluster-master0 redis-cli \
   -p 6379 \
   --user cluster --pass clusterpwd \
   cluster info
   ```

5. Cluster Replicate

   ```bash
   $ while IFS="" read -r ip
   do
   	docker exec cluster-master0 redis-cli \
   	-p 6379 \
   	--user cluster --pass clusterpwd \
   	cluster nodes | grep $ip | awk '{printf $1}' >> ./masteridlist && echo >> ./masteridlist
   	
   done < masteriplist
   
   # Cluster Replicate
   $ paste slavelist masteridlist | while read -r slave masterid
   do
   	docker exec $slave redis-cli \
   	-p 6379 \
   	--user cluster --pass clusterpwd \
   	cluster replicate $masterid
   
   done
   
   # Verify
   $ docker exec cluster-master0 redis-cli \
   -p 6379 \
   --user cluster --pass clusterpwd \
   cluster nodes
   ```

6. Enjoy :smile:

7. (Clean Up)

   ```bash
   $ rm masteridlist && docker compose stop && rm -rf ./tmp/*
   ```