# Prometheus HA Monitoring Stack 🚀

A high-availability monitoring stack built using **Prometheus**, **HAProxy**, **Grafana**, and **Ansible**.  
This setup is deployable on virtual machines using Docker Compose and offers redundancy and fault tolerance for production-grade observability.

---

## 📌 Architecture Overview

Each virtual machine runs a Node Exporter. Two Prometheus replicas independently scrape **all** Node Exporters to ensure full metric redundancy.  
A Federation Prometheus (on the third node) scrapes both replicas.  
HAProxy provides a single endpoint to access the replicas, and Grafana connects through it.

```text
Node Exporters       Node Exporters        Node Exporters
     129                  130                   131
      |                    |                      |
 ┌────┴────┐        ┌──────┴──────┐        ┌───────────────┐
 │Prometheus│        │Prometheus  │        │ Federation     │
 │   129    │        │   130      │        │  Prometheus    │
 └──────────┘        └────────────┘        └───────────────┘
       \______________________/ \________________________/
                      \              /
                    [    HAProxy    ]
                          |
                       [ Grafana ]
```

---

## 🚀 How to Deploy

```text
📌 Prerequisites:
- Ansible installed on your local machine
- Docker & Docker Compose installed on all target VMs
- SSH access from control machine to all VMs (configured in `hosts.ini`)
```

---

## 📂 Step-by-Step

```bash
# 1. Clone the repository
git clone https://github.com/your-username/prometheus-ha.git
cd prometheus-ha

# 2. Update your inventory file
nano inventories/dev/hosts.ini
# Modify it with your VM IPs and SSH access

# 3. Run the Ansible playbook
ansible-playbook -i inventories/dev/hosts.ini playbook.yml
```

```text
This will:
- Generate Prometheus, HAProxy, and Grafana configuration files
- Deploy Docker Compose services on each node
- Start and enable all monitoring containers
```

---

## 📊 Grafana Setup

```text
- Access Grafana at: http://<grafana-ip>:3000
- Default login: admin / admin
- Add a Prometheus data source pointing to: http://<haproxy-ip>:9091
- Import the "Node Exporter Full" dashboard or use the provided JSON
```

---

## ✅ SRE Readiness Checklist

```text
- [x] Prometheus replicas scrape all Node Exporters (redundancy)
- [x] HAProxy load balances Prometheus access (single endpoint)
- [x] Federation Prometheus aggregates remote metrics
- [x] Grafana visualizes data via HAProxy
- [x] Ansible deploys everything automatically
```

---

## 🪪 License

```text
MIT — Free to use, modify, and distribute.
Attribution appreciated but not required.
```

