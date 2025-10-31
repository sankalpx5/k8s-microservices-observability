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




### 📸 Screenshots 
<img width="1858" height="470" alt="Screenshot 2025-10-31 013458" src="https://github.com/user-attachments/assets/fce5d886-5b0c-457e-8628-a3a26f816acc" />  <img width="1863" height="404" alt="Screenshot 2025-10-31 013511" src="https://github.com/user-attachments/assets/5c802ef8-9e92-4325-9617-696dcc8134eb" />
<img width="1861" height="446" alt="Screenshot 2025-10-31 013521" src="https://github.com/user-attachments/assets/76b38527-4550-4c41-bdf4-474d15464585" />
<img width="1859" height="603" alt="Screenshot 2025-10-31 013532" src="https://github.com/user-attachments/assets/01165b03-ff74-492e-98e4-e9410909d46d" />


