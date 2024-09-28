---
title: "Phase 4c : Monitoring | Monitor Jenkins Dashboard"
datePublished: Sat Sep 28 2024 17:52:19 GMT+0000 (Coordinated Universal Time)
cuid: cm1mg8xmg00020ajrflxrf3cg
slug: phase-4c-monitoring-monitor-jenkins-dashboard
tags: grafana, prometheus-installation

---

### Add prometheus metric plugin in jenkins

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727541965742/835c7812-1e9a-4949-9bbc-4a6be0d69a36.png align="center")

Apply below changes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727542175659/da34daea-5739-47ca-87bb-143e4dacc526.png align="center")

We need to add jenkins prometheus path and port in prometheus.yml file.

In the monitoring server, do

```plaintext
cd /etc/prometheus
sudo nano prometheus.yml
```

Add below configurations in this file

```plaintext
 - job_name: "jenkins"
    metrics_path: '/prometheus'
    static_configs:
      - targets: ["jenkinsserver:8080"]
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727542613162/31480c1e-d822-468b-8184-ff1dde7d1bf6.png align="center")

Access metrics thru jenkinsserveipaddress:8080/prometheus

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727542963969/dd192216-fc8a-4347-ad32-bfdc5dfa1bcf.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727542755385/25e3dfcf-b4a1-4059-91fe-3a79dfa8c4e8.png align="center")

### Import Jenkins grafana dashboard

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727543843454/1c19258a-af37-476c-947d-7dd9eb298984.png align="center")

id:9964

This is the jenkins dahsboard looks like

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1727543960731/f576caff-8fa4-4d59-bb52-ad33d29b4fb0.png align="center")

run the build again and check the dashboard.