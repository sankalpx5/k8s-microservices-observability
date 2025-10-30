# Microservices Demo — Kubernetes + Grafana Cloud Monitoring

###  Overview
This project implements **Google’s microservices-demo (Online Boutique)** on a **Kubernetes cluster** using **Minikube**, with **Grafana Cloud** integration for complete observability.  

It provides visibility into:
- Kubernetes cluster metrics  
- Application-level metrics  
- Pod and application logs (via Loki)

---

##  Architecture

**Components**
- **Kubernetes (Minikube)**: Local K8s cluster for hosting microservices  
- **Microservices Demo App**: Google’s 11-service Online Boutique  
- **Load Generator**: Simulates real traffic  
- **Grafana Cloud**: Observability platform (Prometheus + Loki)  
- **Grafana Alloy (Agent)**: For collecting and pushing logs/metrics to Grafana Cloud  

**Flow**
1. Microservices run inside Kubernetes.  
2. Grafana Alloy collects metrics and logs.  
3. Data is pushed to Grafana Cloud.  
4. Dashboards visualize performance, health, and logs in real time.  

---

##  Setup Steps

### 1️⃣ Start Minikube
```bash
minikube start --cpus=4 --memory=8192
```

---

### 2️⃣ Deploy Microservices Demo
```bash
git clone https://github.com/GoogleCloudPlatform/microservices-demo.git
cd microservices-demo
kubectl apply -f ./kubernetes-manifests/
```

---

### 3️⃣ Generate Traffic
```bash
kubectl apply -f ./kubernetes-manifests/loadgenerator.yaml
```
This simulates continuous user activity.

---

## 📊 Grafana Cloud Integration

### 4️⃣ Configure Grafana Cloud
- Created a Grafana Cloud account  
- Retrieved credentials for Prometheus (metrics) and Loki (logs)  

---

### 5️⃣ Install Grafana Alloy (Agent) via Helm
Installed using Helm:
```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm upgrade --install grafana-k8s-monitoring grafana/k8s-monitoring \
  --namespace default --create-namespace --values grafana-agent-values.yaml
```

✅ Data from Minikube cluster now flows to Grafana Cloud.

---

## 🖥️ Grafana Dashboard Setup

- Imported dashboards from Grafana Labs (IDs: **315**, **6417**, **15759**)  
- Created a **custom dashboard** showing:
  - CPU & Memory usage per node  
  - Pod count by namespace  
  - Disk usage %  
  - Live application logs via Loki  

**Example CPU Query**
```promql
sum(rate(container_cpu_usage_seconds_total[5m])) by (instance)
```

**Logs Query**
```logql
{namespace="default"}
```

**Dashboard Title:**  
> Kubernetes Cluster - Microservices Demo  

---

## 🔍 Verification

| Feature | Verification |
|----------|---------------|
| Metrics | Visible in Grafana → Explore → Prometheus |
| Logs | Visible in Grafana → Explore → Loki |
| Cluster Status | Connected in Grafana Cloud → Connections → Kubernetes |
| Load Generator | Produces continuous traffic |

---

## 🧠 Learnings
This was my **first hands-on experience with Kubernetes and Grafana**, and through this project I:  
- Deployed a microservices app end-to-end  
- Integrated Grafana Cloud monitoring & logging  
- Built a custom observability dashboard from scratch  

---


---

### 📸 Screenshots (Optional)
*(Replace below lines with your actual screenshots)*  
- `![Dashboard Overview](screenshots/dashboard-overview.png)`  
- `![Logs View](screenshots/logs.png)`  
