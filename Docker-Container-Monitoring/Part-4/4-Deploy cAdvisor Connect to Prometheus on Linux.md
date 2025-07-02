
# 🚀 Step-by-Step: Deploy cAdvisor and Connect to Prometheus

### 🧱 1. Run cAdvisor as a Container

Fire it up with Docker:

```bash
docker run -d \
  --name=cadvisor \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:ro \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  --detach \
  gcr.io/cadvisor/cadvisor:latest
```

This maps all the necessary volumes so cAdvisor can introspect your host and containers, and exposes the web UI on `http://localhost:8080`.

---

### 📡 2. Update Prometheus to Scrape cAdvisor

In your `prometheus.yml`, add the following under `scrape_configs`:

```yaml
- job_name: 'cAdvisor'
  static_configs:
    - targets: ['localhost:8080']
```

Then reload or restart Prometheus:

```bash
sudo systemctl reload prometheus
# or
sudo systemctl restart prometheus
```

Verify it’s working by visiting Prometheus at `http://localhost:9090` → **Status > Targets**. You should see a `cadvisor` job listed and marked **UP**.

---

### 🎨 3. (Optional) Import a Grafana Dashboard

Grafana has excellent community dashboards that use cAdvisor metrics.

**Recommended Dashboard ID:** `193`  
Title: *“Docker Containers (cAdvisor)”*

To import:
1. Open Grafana → “+” → **Import**
2. Enter **193**, click **Load**
3. Choose your Prometheus data source, then **Import**

You’ll get panels for:
- Container CPU & memory usage
- Filesystem and network I/O
- Real-time container status

---

That’s it—your observability just leveled up from engine stats to per-container insight.
