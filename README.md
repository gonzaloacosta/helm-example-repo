# Create helm chart package

### Create helm chart
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
```

### Test helm chart

```
kubectl get all -n my-app
kubectl exec -it -n my-app my-app-name-69dbd9db6b-qs49x -- apt update
kubectl exec -it -n my-app my-app-name-69dbd9db6b-qs49x -- apt install -y curl
kubectl exec -it -n my-app my-app-name-69dbd9db6b-qs49x -- curl http://localhost
helm uninstall my-app-name -n my-app
```

### Create repo in github pages

```
git clone https://github.com/gonzaloacosta/helm-example-repo.git
cd helm-example-repo
git checkout -b gh-pages
touch index.yaml
git commit -a -m "add index.yaml"
git push --set-upstream origin gh-pages
helm package my-app
mv my-app-0.1.0.tgz helm-example-repo/
helm repo index helm-example-repo/ --url https://gonzaloacosta.github.io/helm-example-repo
cat helm-example-repo/index.yaml
git add .
git commit -a -m "change index"
git push origin
curl https://gonzaloacosta.github.io/helm-example-repo/index.yaml
```

### Add repo and install helm chart

```
helm repo add helm-example-repo https://gonzaloacosta.github.io/helm-example-repo
helm install helm-example-repo/my-app --name=my-app-name
kubectl get pods -n my-app
```


