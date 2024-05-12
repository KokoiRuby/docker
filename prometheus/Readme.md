A quickstart of deploying a [Prometheus](https://prometheus.io/) + [Node-Exporter](https://github.com/prometheus/node_exporter) using [Docker Compose](https://docs.docker.com/compose/) :whale2:

## IP Plan

| Instance      | IP            | Port | HostPort |
| ------------- | ------------- | ---- | -------- |
| Prometheus    | 192.168.40.10 | 9090 | 19090    |
| Node-Exporter | 192.168.40.11 | 9100 | 19100    |

## Steps

2. Docker Compose Up

   ```bash
   $ docker compose up -d
   ```

3. Verify

   - http://localhost:19090/
     - Status → Targets (see more in ./config/prometheus.yml)
     - Alerts (see more in ./rule_files/node-exporter.yml)
   - http://localhost:19090/metrics
   - http://localhost:19100/metrics
   
4. Enjoy :smile:

5. (Clean up)

   ```bash
   $ docker compose stop
   ```

   

   

