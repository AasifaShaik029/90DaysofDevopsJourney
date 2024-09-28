---
title: "Phase 4a : Monitoring | Set up Prometheus and Grafana to monitor your application | Prometheus and Node Exporter Setup"
datePublished: Sat Sep 28 2024 12:31:36 GMT+0000 (Coordinated Universal Time)
cuid: cm1m4si5q000108l74wnvcjif
slug: phase-4a-monitoring-set-up-prometheus-and-grafana-to-monitor-your-application-prometheus-and-node-exporter-setup
tags: monitoring-using-prometheus-and-grafana-on-aws-ec2

---

## Launch Another server T2.Medium and attach elastic IP

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727511473458/f3677a92-a3f0-4a77-8efd-a086a0540463.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727511695798/e1d0ea79-647b-435d-87c4-ddd8ca1f05ff.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727511737698/c90a3069-dfb8-4c69-a0d1-0c5f267536b5.png align="center")

### Ports Required

9090

# Setting up **prometheus and Node Exporter**

### **Installing Prometheus**

First, create a dedicated Linux user for Prometheus and download Prometheus:

```plaintext
sudo useradd --system --no-create-home --shell /bin/false prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.47.1/prometheus-2.47.1.linux-amd64.tar.gz
```

Extract Prometheus files, move them, and create directories:

```plaintext
tar -xvf prometheus-2.47.1.linux-amd64.tar.gz
cd prometheus-2.47.1.linux-amd64/
sudo mkdir -p /data /etc/prometheus
sudo mv prometheus promtool /usr/local/bin/
sudo mv consoles/ console_libraries/ /etc/prometheus/
sudo mv prometheus.yml /etc/prometheus/prometheus.yml
```

Set ownership for directories:

```plaintext
sudo chown -R prometheus:prometheus /etc/prometheus/ /data/
```

Create a systemd unit configuration file for Prometheus:

```plaintext
sudo nano /etc/systemd/system/prometheus.service
```

Add the following content to the `prometheus.service` file:

```plaintext
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=prometheus
Group=prometheus
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/data \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries \
  --web.listen-address=0.0.0.0:9090 \
  --web.enable-lifecycle

[Install]
WantedBy=multi-user.target
```

Here's a brief explanation of the key parts in this `prometheus.service` file:

* `User` and `Group` specify the Linux user and group under which Prometheus will run.
    
* `ExecStart` is where you specify the Prometheus binary path, the location of the configuration file (`prometheus.yml`), the storage directory, and other settings.
    
* `web.listen-address` configures Prometheus to listen on all network interfaces on port 9090.
    
* `web.enable-lifecycle` allows for management of Prometheus through API calls.
    

Enable and start Prometheus:

```plaintext
sudo systemctl enable prometheus
sudo systemctl start prometheus
```

Verify Prometheus's status:

```plaintext
sudo systemctl status prometheus
```

You can access Prometheus in a web browser using your server's IP and port 9090:

`http://<your-server-ip>:9090`

Our prometheus is now running.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727514149201/c2d051d7-a0df-4d04-971c-72aa1947e034.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727514163029/cfa0eaa7-c979-41dc-974e-d1e8b1a30897.png align="center")

## Setting up Node Exporter

### **Installing Node Exporter**

```plaintext
sudo useradd --system --no-create-home --shell /bin/false node_exporter
```

Extract Node Exporter files, move the binary, and clean up:

```plaintext
tar -xvf node_exporter-1.6.1.linux-amd64.tar.gz
sudo mv node_exporter-1.6.1.linux-amd64/node_exporter /usr/local/bin/
rm -rf node_exporter*
```

Create a systemd unit configuration file for Node Exporter:

```plaintext
sudo nano /etc/systemd/system/node_exporter.service
```

Add the following content to the `node_exporter.service` file:

```plaintext
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter --collector.logind

[Install]
WantedBy=multi-user.target
```

Replace `--collector.logind` with any additional flags as needed.

Enable and start Node Exporter:

```plaintext
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
```

Verify the Node Exporter's status:

```plaintext
sudo systemctl status node_exporter
```

You can access Node Exporter metrics in Prometheus.

## **Configure Prometheus Plugin Integration**

Integrate Jenkins with Prometheus to monitor the CI/CD pipeline.

### **Prometheus Configuration**

To configure Prometheus to scrape metrics from Node Exporter and Jenkins, you need to modify the `prometheus.yml` file. Here is an example `prometheus.yml` configuration for your setup:

```plaintext
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "node_exporter"
    static_configs:
      - targets: ["3.223.103.7:9100"]
```

Replace 3.223.103.7:9100 with your monitoring server IP address.

Check the validity of the configuration file:

```plaintext
promtool check config /etc/prometheus/prometheus.yml
```

Reload the Prometheus configuration without restarting:

```plaintext
curl -X POST http://localhost:9090/-/reload
```

You can access Prometheus targets at:

`http://<your-prometheus-ip>:9090/targets`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727522201346/969de528-b896-4406-bcd0-eb51f3ce5bf9.png align="center")

When you access on 9100 port you can see the metrics

[http://3.223.103.7:9100/metrics](http://3.223.103.7:9100/metrics)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727523571364/f0deb387-2275-43eb-8627-cf6a39038110.png align="center")