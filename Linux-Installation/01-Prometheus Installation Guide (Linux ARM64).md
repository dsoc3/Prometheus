# 🚀 Prometheus Installation Guide (Linux ARM64)

This guide walks you through installing Prometheus on a Linux-based ARM64 system such as Raspberry Pi or Apple Silicon.

---

## 🧑‍💻 Step 1: Create Prometheus User and Directory

Create a dedicated non-login user and the necessary directories for configuration and data.

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
sudo mkdir -p /etc/prometheus /var/lib/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
```

---

## 📦 Step 2: Download and Extract Prometheus

Fetch the ARM64 binary archive and unpack it.

```bash
cd /tmp
wget https://github.com/prometheus/prometheus/releases/download/v3.4.1/prometheus-3.4.1.linux-arm64.tar.gz
tar -xvf prometheus-3.4.1.linux-arm64.tar.gz
cd prometheus-3.4.1.linux-arm64
```

---

## ⚙️ Step 3: Install Binaries and Example Configuration

Move Prometheus files into system-wide locations and prepare configuration folders.

```bash
sudo mv prometheus promtool /usr/local/bin/
sudo chmod +x /usr/local/bin/prometheus /usr/local/bin/promtool
sudo mv prometheus.yml /etc/prometheus/
sudo mkdir -p /etc/prometheus/consoles /etc/prometheus/console_libraries
sudo chown -R prometheus:prometheus /etc/prometheus
```

---

## 🧾 Step 4: Create systemd Service

Create a systemd service file so Prometheus can run in the background and start on boot.

```bash
sudo vim /etc/systemd/system/prometheus.service
```

Paste the following into the file:

```ini
[Unit]
Description=Prometheus Monitoring
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

Save and close the file.

---

## ▶️ Step 5: Start and Enable Prometheus

Enable the Prometheus service so it starts on boot and run it now.

```bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
```

---

## 🌐 Step 6: Verify the Setup

Open your browser and navigate to:

```
http://localhost:9090
```

You should see the Prometheus dashboard running. 🎉
