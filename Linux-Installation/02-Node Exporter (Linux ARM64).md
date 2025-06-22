# 🚀 Node Exporter Installation Guide (Linux ARM64)

This guide walks you through installing Node Exporter on a Linux-based ARM64 system such as Raspberry Pi or Apple Silicon.

---

## 🧑‍💻 Step 1: Create Node Exporter User

Create a dedicated non-login user for running the exporter.

```bash
sudo useradd --no-create-home --shell /bin/false node_exporter
```

---

## 📦 Step 2: Download and Extract Node Exporter

Fetch the ARM64 binary and unpack it.

```bash
cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-arm64.tar.gz
tar -xvf node_exporter-1.9.1.linux-arm64.tar.gz
cd node_exporter-1.9.1.linux-arm64
```

---

## ⚙️ Step 3: Install Binary

Move the binary to your system's executable path and assign ownership.

```bash
sudo mv node_exporter /usr/local/bin/
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

---

## 🧾 Step 4: Create systemd Service

Set up a systemd service to manage Node Exporter.

```bash
sudo nano /etc/systemd/system/node_exporter.service
```

Paste the following configuration:

```ini
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

Save and exit the file.

---

## ▶️ Step 5: Start and Enable Node Exporter

Start the service and enable it on boot.

```bash
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
```

---

## 🌐 Step 6: Verify the Setup

Open your browser and navigate to:

```
http://localhost:9100/metrics
```

You should see the updated Node Exporter metrics streaming. 🔍
