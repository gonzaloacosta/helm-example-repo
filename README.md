# Create helm chart package

```
gh repo create --public helm-example-repo
gh repo clone helm-example-repo
cd helm-example-repo

helm create my-app

cat << HELM > my-app/templates/cm.yaml
apiVersion: v1
data:
  nginx.conf: |
    events {
      worker_connections  1024;
    }
    http {
      server {
        listen 80;
        location / {
          return 200 "===============================\n\n   This is your helm deploy!   \n\n===============================\n";
        }
      }
    }
kind: ConfigMap
metadata:
  name: nginx-config
HELM

helm template my-app
helm install --namespace my-app --create-namespace my-app-name my-app
kubectl get all -n my-app
kubectl exec -it -n my-app my-app-name-69dbd9db6b-qs49x -- apt update
kubectl exec -it -n my-app my-app-name-69dbd9db6b-qs49x -- apt install -y curl
kubectl exec -it -n my-app my-app-name-69dbd9db6b-qs49x -- curl http://localhost
```
