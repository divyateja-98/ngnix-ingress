# ngnix-ingress

# Exposing NGINX via Ingress and Custom Domain on AWS EKS

This guide demonstrates how to:

- Install the NGINX Ingress Controller on an AWS EKS cluster.
- Deploy an NGINX application.
- Expose the application using an Ingress resource.
- Configure a custom domain in AWS Route 53 to point to the Ingress Controller.

## Prerequisites

- AWS CLI configured with appropriate credentials.
- `kubectl` configured to interact with your EKS cluster.
- Helm installed in your environment.
- A registered domain name managed via AWS Route 53.

## Steps

1. Clone the Repository

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>


2.(a)Install NGINX Ingress Controller
Add the ingress-nginx Helm repository:

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
(b)Install the NGINX Ingress Controller

    1.kubectl create namespace ingress-nginx

    2. helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --set controller.service.type=LoadBalancer
   Wait for the LoadBalancer IP to be assigned:

kubectl get services -n ingress-nginx
Note the EXTERNAL-IP of the ingress-nginx-controller service.

3.Deploy NGINX Application
Apply the deployment and service manifests:

kubectl apply -f manifests/nginx-deployment.yaml
kubectl apply -f manifests/nginx-service.yaml
4. Create Ingress Resource
Apply the Ingress manifest:

kubectl apply -f manifests/nginx-ingress.yaml
5. Configure AWS Route 53
In the AWS Route 53 console:

Navigate to your hosted zone.

Create a new record:

Record name: nginx.example.com (replace with your domain)

Record type: A – IPv4 address

Alias: Yes

Alias Target: Select the LoadBalancer DNS name of the ingress-nginx-controller service.
Verify DNS Resolution
Use nslookup to verify DNS resolution:

nslookup nginx.example.com

Access the Application
Open your browser and navigate to http://nginx.example.com. You should see the default NGINX welcome page.

for clean up
kubectl delete -f manifests/nginx-ingress.yaml
kubectl delete -f manifests/nginx-service.yaml
kubectl delete -f manifests/nginx-deployment.yaml
helm uninstall ingress-nginx -n ingress-nginx
kubectl delete namespace ingress-nginx

