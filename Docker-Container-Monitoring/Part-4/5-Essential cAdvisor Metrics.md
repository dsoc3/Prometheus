# 🚀 Essential cAdvisor Metrics for Beginners

### 1. `cadvisor_version_info`
**Type:** `gauge`  
**What it is:** A metadata metric that always equals `1`, but its labels reveal key details:
- Kernel version
- OS version
- cAdvisor version
- Docker version

🛠️ **Use it for:** Displaying environment info in dashboards.

**Sample Query (PromQL):**
```promql
cadvisor_version_info
```

---

### 2. `container_cpu_usage_seconds_total`
**Type:** `counter`  
**What it is:** Total CPU time (in seconds) consumed by a container.

🛠️ **Use it for:** Tracking CPU usage over time.

**Sample Query (PromQL):**
```promql
rate(container_cpu_usage_seconds_total[5m])
```
This gives a per-second CPU rate over 5 minutes—perfect for Grafana dashboards.

---

### 3. `container_memory_usage_bytes`
**Type:** `gauge`  
**What it is:** Current memory usage of each container in bytes.

🛠️ **Use it for:** Understanding how much memory each container consumes.

**Sample Query (in GB):**
```promql
container_memory_usage_bytes / 1024 / 1024 / 1024
```

---

### 4. `container_network_receive_bytes_total` & `container_network_transmit_bytes_total`
**Type:** `counter`  
**What they are:** Total bytes received and transmitted by a container.

🛠️ **Use it for:** Network traffic monitoring per container.

**Sample Query (rate in bytes/sec):**
```promql
rate(container_network_receive_bytes_total[1m])
rate(container_network_transmit_bytes_total[1m])
```

---

### 5. `container_blkio_device_usage_total`
**Type:** `counter`  
**What it is:** Total I/O (bytes) read or written on block devices by containers.

🛠️ **Use it for:** Identifying containers with heavy disk usage.

**Sample Query (read/write over time):**
```promql
rate(container_blkio_device_usage_total{operation="Read"}[1m])
rate(container_blkio_device_usage_total{operation="Write"}[1m])
```

---

## 🧠 Tips for Beginners

- Look at **labels like `name` or `id`** to identify which container the metric refers to.
- Use **Grafana dashboards** to visualize these in real time.
- Always prefer `rate()` for counters to get a sense of change over time.

---
