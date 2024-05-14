A quickstart of deploying a **remote-read** [Prometheus](https://prometheus.io/) using [Docker Compose](https://docs.docker.com/compose/) :whale2:

## IP Plan

| Instance           | IP               | Port | HostPort |
| ------------------ | ---------------- | ---- | -------- |
| Grafana            | 192.168.40.14/16 | 3000 | 13000    |
| Prometheus_Reader  | 192.168.40.15/16 | 9090 | 19090    |
| Prometheus_Scraper | 192.168.40.16/16 | 9090 | 29090    |
| Node-Exporter      | 192.168.40.17/16 | 9100 | 19100    |

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

   - http://localhost:13000/
     - admin/admin
   - http://localhost:19090/
   - http://localhost:29090/

4. Enjoy :smile:

5. (Clean up)

   ```bash
   $ docker compose stop
   ```

   

   

