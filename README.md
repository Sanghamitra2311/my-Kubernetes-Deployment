# my-Kubernetes-Deployment
This project contains Kubernetes manifests for deploying a simple application (e.g., NGINX) on a Google Kubernetes Engine (GKE) Autopilot cluster. The app uses a ConfigMap to externalize configuration and is exposed internally via a ClusterIP service.


📁 Project Structure

my-Kubernetes-Deployment/
├── configmap.yaml      # ConfigMap containing environment variables
├── deployment.yaml     # Deployment manifest using the ConfigMap
├── service.yaml        # ClusterIP service for internal exposure
└── README.md           # Documentation


🚀 What This Project Does
Creates a ConfigMap to store environment variables (APP_ENV, LOG_LEVEL, etc.)

Deploys an NGINX container in an Autopilot-managed pod

Injects environment variables into the pod using envFrom

Exposes the pod internally using a ClusterIP Kubernetes Service

🧰 Prerequisites
A GKE Autopilot cluster (private or public)

Google Cloud SDK installed and authenticated

kubectl CLI installed and configured

Access to the GitHub repo (this one)

☁️ GKE Setup (Autopilot)
Authenticate with GKE:
gcloud config set project ascension-poc-prj
gcloud container clusters get-credentials gke-autopilot-private --region us-central1

📄 Apply Manifests

Run the following from the root of the project folder:
kubectl apply -f configmap.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

🔍 Verify Deployment
Check that the pods are running:
kubectl get pods

Check the ConfigMap is applied:
kubectl get configmap app-config -o yaml

Test internal access from a temporary pod:
kubectl run test-shell --rm -i -t --image=alpine -- sh

# Inside the shell:
wget -qO- http://demo-service

🧪 Environment Variables Injected
From the ConfigMap, the following are available as env vars in the container:

APP_ENV: production

LOG_LEVEL: info

📌 Notes
This project is designed for GKE Autopilot clusters.

The service is not exposed externally by default (ClusterIP only).

To expose it publicly, you can modify service.yaml to use a LoadBalancer or set up Ingress (not recommended for private clusters unless using Internal Load Balancer).

🔒 Security
Avoid hardcoding secrets in ConfigMaps. For sensitive data, use Kubernetes Secrets.

Follow IAM best practices for GKE Autopilot access.

🛠️ Future Enhancements
Add GitHub Actions CI/CD for automatic deployment

Add Ingress support

Add Prometheus monitoring annotations

📬 Author
Sanghamitra Ghosh
Email: sanghamitrag239@gmail.com
GitHub: Sanghamitra2311

