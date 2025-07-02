# 🎨 Step-by-Step: Create a Custom Grafana Dashboard for Docker Engine Metrics

### ✅ Prerequisites
- Prometheus is running and scraping Docker metrics (e.g., from `localhost:9323`)
- Grafana is installed and accessible (usually at `http://localhost:3000`)
- Prometheus is added as a data source in Grafana

---

### 🧭 Step 1: Log in and Create a New Dashboard
1. Open Grafana in your browser.
2. Log in (default: `admin` / `admin`).
3. Click the **“+”** icon on the left sidebar → **Dashboard** → **New Dashboard**.
4. Click **“Add a new panel”**.

---

### 📊 Step 2: Add Panels for Each Metric

#### 🧠 Panel 1: Docker Engine Info
- **Query**:
  ```promql
  engine_daemon_engine_info
  ```
- **Visualization**: Use **Table** or **Stat** panel.
- **Tip**: In the **Labels** section, display labels like `version`, `architecture`, `operating_system`.

---

#### 🧮 Panel 2: CPU Count
- **Query**:
  ```promql
  engine_daemon_engine_cpus_cpus
  ```
- **Visualization**: **Stat**
- **Unit**: `none` or `short`

---

#### 💾 Panel 3: Total Memory (in GB)
- **Query**:
  ```promql
  engine_daemon_engine_memory_bytes / 1073741824
  ```
- **Visualization**: **Stat**
- **Unit**: `bytes (IEC)` → `GB`

---

#### 📦 Panel 4: Container States
- **Query**:
  ```promql
  engine_daemon_container_states_containers
  ```
- **Visualization**: **Bar Gauge** or **Pie Chart**
- **Legend**: Use the `state` label to group by container state (e.g., running, stopped)

---

#### 🧩 Step 3: Customize and Save
- Add titles to each panel (e.g., “Docker Version Info”, “Total CPUs”, etc.)
- Adjust refresh interval (top-right corner) to something like `10s` or `30s`
- Click **Apply** to save each panel
- Once all panels are added, click **Save dashboard**, give it a name like `Docker Engine Overview`, and hit **Save**

---

#### 🧠 Bonus: Add a Row for System Info (Optional)
You can add a row with panels for:
- `node_memory_MemTotal_bytes` (if using node-exporter)
- `process_resident_memory_bytes` (for Prometheus memory)
- `up` (to show target health)

---
