# argocd-autopilot-test

ArgoCD autopilot test

## Initial setup

Get Arkade, create a repository scoped Github Token

```
arkade get kind
arkade get kubectl
arkade get argocd-autopilot
export GIT_TOKEN=ghp_XXXXXXXXX
export GIT_REPO=https://github.com/rgarrigue/argocd-autopilot-test
argocd-autopilot repo bootstrap

screen -dmS kpf8080 kubectl port-forward -n argocd svc/argocd-server 8080:80
```

Copy the password given at the end of the bootstrap, then head to http://localhost:8080, admin / <password>

## Iterate

Wipe cluster & restart

```
kind delete cluster
kind create cluster
argocd-autopilot repo bootstrap --recover
```
