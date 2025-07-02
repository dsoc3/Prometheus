# 🐳 Monitoring Docker Daemon with Prometheus on Linux (No cAdvisor)
---

Monitor Docker's internal performance using Prometheus without any external exporters—just the Docker Engine and Prometheus.

---

### 🔧 Step 1: Enable Docker Metrics Endpoint

Edit or create `/etc/docker/daemon.json`:

```json
{
  "metrics-addr": "127.0.0.1:9323",
  "experimental": true
}
```

Then restart Docker:

```bash
sudo systemctl restart docker
```

> Docker will now expose metrics at [http://localhost:9323/metrics](http://localhost:9323/metrics)

---

### 🧪 Step 2: Confirm Experimental Mode is Active

```bash
docker info | grep Experimental
```

You should see:

```
Experimental: true
```

If not, double-check the `daemon.json` format and restart Docker again.

---

### 📡 Step 3: Configure Prometheus to Scrape Docker Metrics

Open `/etc/prometheus/prometheus.yml` and add:

```yaml
scrape_configs:
  - job_name: 'Docker'
    static_configs:
      - targets: ['localhost:9323']
```

Then reload or restart Prometheus:

```bash
sudo systemctl reload prometheus
# or
sudo systemctl restart prometheus
```

---

### ✅ Step 4: Verify Everything Is Working

Visit Prometheus UI:  
[http://localhost:9090/targets](http://localhost:9090/targets)

You should see the `Docker` job marked as **UP**.

Try this query to view available Docker metrics:

```promql
{__name__=~"engine_.*"}
```

If it returns results, your setup is solid!

---

### 📊 Docker Daemon Metrics You Might See

| Metric Name                           | Description                                 |
|--------------------------------------|---------------------------------------------|
| `engine_daemon_engine_memory_bytes`  | Total memory visible to Docker              |
| `engine_daemon_engine_cpus_cpus`     | Number of logical CPU cores                 |
| `engine_daemon_container_states_containers` | Count of running, paused, or stopped containers |
| `engine_daemon_cpu_seconds_total`    | Docker daemon's CPU usage over time         |
| `engine_daemon_memstats_total_rss`   | Memory used by the Docker daemon itself     |
| `engine_daemon_engine_info`          | Docker version and host system info         |

---
