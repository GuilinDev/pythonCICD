# PythonCICD demo on Mac

### install k8s cluster by kind
```shell

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