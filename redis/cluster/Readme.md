A quickstart of deploying a redis cluster using [Docker Compose](https://docs.docker.com/compose/) :whale2:

## IP Plan

- Sentinel{0..2} → Master{0..2} ← Slave{0..2}
- Credentials of redis-cli: `cluster/clusterpwd`, see more in redis.conf

| Instance  | IP            | Port | HostPort |
| --------- | ------------- | ---- | -------- |
| Master0   | 192.168.20.10 | 6379 | 16380    |
| Master1   | 192.168.20.11 | 6379 | 16381    |
| Master2   | 192.168.20.12 | 6379 | 16382    |
| Slave0    | 192.168.20.20 | 6379 | 26380    |
| Slave1    | 192.168.20.21 | 6379 | 26381    |
| Slave2    | 192.168.20.22 | 6379 | 26382    |
| Sentinel0 | 192.168.20.30 | 6379 | 36380    |
| Sentinel1 | 192.168.20.31 | 6379 | 36381    |
| Sentinel2 | 192.168.20.32 | 6379 | 36382    |

## Steps

1. Docker Compose Up

   ```bash
   $ docker compose up
   ```

2. Cluster Meet

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

3. Add Slots

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

4. Cluster Replicate

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

5. Enjoy :smile:

6. (Clean Up)

   ```bash
   $ rm masteridlist && docker compose stop && rm -rf ./tmp/*
   ```