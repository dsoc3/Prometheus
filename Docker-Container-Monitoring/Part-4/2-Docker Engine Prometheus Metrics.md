# 🔍 Understanding Docker Engine Prometheus Metrics (with Linux Equivalents)

These metrics are exposed by the Docker daemon when Prometheus scraping is enabled (via `metrics-addr` in `daemon.json`). They provide insight into the Docker Engine’s configuration and container state.

---

### 1. **`engine_daemon_engine_info`**

**What it is:**  
A static info metric that exposes metadata about the Docker Engine, such as version, architecture, OS, and more. It’s a *gauge* with constant value `1` and labels that carry the actual data.

**Sample Prometheus Output:**
```
engine_daemon_engine_info{architecture="x86_64", cpus="8", kernel_version="6.5.0", operating_system="Ubuntu 22.04", version="24.0.5"} 1
```

**Linux Equivalent:**
```bash
docker info
```

Or for more granular system info:
```bash
uname -a
lsb_release -a
lscpu
```

---

### 2. **`engine_daemon_engine_cpus_cpus`**

**What it is:**  
Reports the number of logical CPUs available to the Docker Engine.

**Sample Prometheus Output:**
```
engine_daemon_engine_cpus_cpus 8
```

**Linux Equivalent:**
```bash
nproc
# or
lscpu | grep "^CPU(s):"
```

---

### 3. **`engine_daemon_engine_memory_bytes`**

**What it is:**  
Total memory (in bytes) available to the Docker Engine.

**Sample Prometheus Output:**
```
engine_daemon_engine_memory_bytes 33554432000
```

**Linux Equivalent (in GB):**
```bash
free -g | awk '/^Mem:/ {print $2 " GB"}'
# or
grep MemTotal /proc/meminfo | awk '{printf "%.2f GB\n", $2/1024/1024}'
```

---

### 4. **`engine_daemon_container_states_containers`**

**What it is:**  
Shows the number of containers in each state (e.g., running, paused, stopped).

**Sample Prometheus Output:**
```
engine_daemon_container_states_containers{state="running"} 3
engine_daemon_container_states_containers{state="stopped"} 2
```

**Linux Equivalent:**
```bash
docker ps -a --format '{{.Status}}' | awk '
/Up/ {running++}
/Exited/ {stopped++}
END {
  print "Running containers: " running
  print "Stopped containers: " stopped
}'
```

---

### 🧩 Bonus Tip

To explore these metrics live, run:
```bash
curl http://localhost:9323/metrics | grep engine_daemon
```
OR via Browser, run:
```bash
http://localhost:9323/metrics 
```

This will show all Docker Engine metrics exposed to Prometheus.

---
