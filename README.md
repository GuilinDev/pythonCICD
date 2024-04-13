# PythonCICD demo on Mac

### install kind
```shell
# check version for kind, 0.22.0 is for kubectl 1.29.2
curl -Lo ./kind https://github.com/kubernetes-sigs/kind/releases/download/v0.22.0/kind-$(uname)-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

### install k8s cluster by kind
Create kind-config.yaml for k8s cluster
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```
apply
```shell
kind create cluster --config kind-config.yaml
```

### install argoCD
```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl port-forward svc/argocd-server -n argocd 8033:443
```

Get default password
```shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

# change default pwd
kubectl -n argocd patch secret argocd-secret \
  -p '{"stringData": {
      "admin.password": "$(htpasswd -bnBC 10 "" newpassword | tr -d ':\n')",
      "admin.passwordMtime": "$(date +%FT%T%Z)"
  }}'

```

### install helm


### access flask app
```shell
# find k8s Loadbalancers EXTERNAL-IP
kubectl get svc python-flask-app-service -n guilindev
# for local k8s cluster installed by minikube or kind, k8s cluster lacks cloud provider for EXTERNAL-IP, need to port forward cluster IP manually
kubectl port-forward svc/python-flask-app-service 5000:80 -n guilindev

# http://localhost:5000/
```

