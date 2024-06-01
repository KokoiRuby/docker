A quickstart of deploying a [Prometheus](https://prometheus.io/) + [Node-Exporter](https://github.com/prometheus/node_exporter) + [Alert-Manager](https://prometheus.io/docs/alerting/latest/alertmanager/) + [Grafana](https://grafana.com/) using [Docker Compose](https://docs.docker.com/compose/) :whale2:

## IP Plan

| Instance      | IP               | Port | HostPort |
| ------------- | ---------------- | ---- | -------- |
| Prometheus    | 192.168.40.10/16 | 9090 | 19090    |
| Node-Exporter | 192.168.40.11/16 | 9100 | 19100    |
| Alert-Manager | 192.168.40.12/16 | 9093 | 19093    |
| Grafana       | 192.168.40.13/16 | 3000 | 13000    |

## Steps

1. Create Network

   ```bash
   $ docker network create --subnet=192.168.0.0/16 local
   ```

2. [TLS setup](https://www.prometheus.io/docs/guides/tls-encryption/) (Done for u)

3. Docker Compose Up

   ```bash
   $ docker compose up -d
   ```

4. Verify

   - https://localhost:19090/
     - Status → Targets (see more in ./config/prometheus/prometheus.yml)
     - Alerts (see more in ./config/prometheus/rule_files/node-exporter.yml)
   - https://localhost:19090/metrics
   - http://localhost:19100/metrics
   - http://localhost:13000
     - admin/admin

5. Setup SMTP (Optional, see more in ./config/alertmanager/alertmanager.yml)

   ```yaml
   # fill in
   global:
     smtp_from: '<ur_email_here>'
     smtp_smarthost: '<smtp_smarthost>'
     smtp_auth_username: '<ur_email_here>'
     smtp_auth_password: '<smtp_authZ_code>'
     
   receivers:
     - name: 'email'
       email_configs:
       - to: '<ur_email_here>'
   ```

6. Enjoy :smile:

7. (Clean up)

   ```bash
   $ docker compose stop
   ```

   

   

