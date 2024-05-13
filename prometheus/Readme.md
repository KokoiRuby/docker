A quickstart of deploying a [Prometheus](https://prometheus.io/) + [Node-Exporter](https://github.com/prometheus/node_exporter) using [Docker Compose](https://docs.docker.com/compose/) :whale2:

## IP Plan

| Instance      | IP            | Port | HostPort |
| ------------- | ------------- | ---- | -------- |
| Prometheus    | 192.168.40.10 | 9090 | 19090    |
| Node-Exporter | 192.168.40.11 | 9100 | 19100    |
| Alert-Manager | 192.168.40.12 | 9093 | 19093    |

## Steps

2. Docker Compose Up

   ```bash
   $ docker compose up -d
   ```

3. Verify

   - http://localhost:19090/
     - Status → Targets (see more in ./config/prometheus/prometheus.yml)
     - Alerts (see more in ./config/prometheus/rule_files/node-exporter.yml)
   - http://localhost:19090/metrics
   - http://localhost:19100/metrics
   
4. Setup SMTP (Optional, see more in ./config/alertmanager/alertmanager.yml)

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

5. Enjoy :smile:

6. (Clean up)

   ```bash
   $ docker compose stop
   ```

   

   

