# ngnix-ingress

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.3.0/deploy/static/provider/cloud/deploy.yaml
kubectl get pods --namespace ingress-nginx
kubectl port-forward --namespace ingress-nginx service/ingress-nginx-controller 8080:80
kubectl create deployment nginx-app --image=nginx
kubectl expose deployment nginx-app --port=80 --target-port=80
Create a file named nginx-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: mydomain.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx-app
            port:
              number: 80

       kubectl apply -f nginx-ingress.yaml
 Replace yourdomain.example.com with your actual domain name
Configure DNS with AWS Route 53
To point your domain to the Ingress Controller, create a CNAME record in Route 53:
serverfault.com

Log in to the AWS Management Console and navigate to Route 53.

Select your hosted zone.

Create a new record:

Record name: mydomain.example.com

Record type: CNAME

Value: The public URL provided by Codespaces for port 8080, typically in the format https://<CODESPACE_NAME>-8080.app.github.dev

This setup directs traffic from yourdomain.example.com to the NGINX application

nslookup yourdomain.example.com


