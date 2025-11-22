# 🐳 Kubernetes MongoDB + Mongo Express Deployment

This project demonstrates how to deploy a **MongoDB database** and **Mongo Express UI** on a **Minikube Kubernetes cluster**, using:

- Deployments  
- ClusterIP & NodePort Services  
- Secrets  
- ConfigMaps  
- Environment variables  
- Pod-to-pod communication  
- Minikube tunnel  

This is a complete beginner–friendly example of container orchestration using Kubernetes.

---

## 📁 Project Structure

                       ┌────────────────────────┐
                       │     Mongo Express      │
                       │  (UI Pod – Deployment) │
                       └────────────┬───────────┘
                                    │
                                    │ NodePort Service (30000)
                                    │
                       ┌────────────────────────┐
                       │      Browser / Host     │
                       └────────────────────────┘

                                    │
                    (internal communication)
                                    │
                       ┌────────────────────────┐
                       │     Mongo Express      │
                       │   reads config from:   │
                       │   - ConfigMap (URL)    │
                       │   - Secret (credentials) 
                       └────────────┬───────────┘
                                    │
                                    │ ClusterIP Service (mongodb-service)
                                    │
                       ┌────────────────────────┐
                       │        MongoDB         │
                       │  (Database Deployment)  │
                       └────────────────────────┘
2️⃣ Apply Secrets
kubectl apply -f mongo-secret.yaml

3️⃣ Apply ConfigMap
kubectl apply -f mongo-configmap.yaml

4️⃣ Deploy MongoDB
kubectl apply -f mongo-deployment.yaml
kubectl apply -f mongo-service.yaml


Check:

kubectl get pods
kubectl get service

5️⃣ Deploy Mongo Express
kubectl apply -f mongo-express-deployment.yaml
kubectl apply -f mongo-express-service.yaml


Check:

kubectl get pods
kubectl get svc

🌐 Access Mongo Express

Open tunnel for NodePort:

minikube service mongo-express-service


This opens:

http://127.0.0.1:<random_port>

You can now view the MongoDB UI in the browser.

🔐 Why Secrets?
Secrets store sensitive values (like DB username/password) securely, instead of hardcoding them in YAML files.

🧩 Why ConfigMaps?

ConfigMaps store non-sensitive configuration, such as:
Database hostname (mongodb-service)
URLs
General application settings

📝 Future Improvements
-Add PersistentVolume for MongoDB (to avoid data loss)
-Add Ingress Controller
-Add monitoring (Prometheus/Grafana)
-Automate with Helm Chart
