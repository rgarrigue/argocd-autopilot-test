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

## Experiment

Tuto style https://argocd-autopilot.readthedocs.io/en/stable/Getting-Started/#add-a-project-and-an-application

```
argocd-autopilot project create dev
argocd-autopilot project create stg
argocd-autopilot project create prod
argocd-autopilot app create hello-world --app github.com/argoproj-labs/argocd-autopilot/examples/demo-app/ -p dev --wait-timeout 2m
```

## Iterate

Wipe cluster & restart

```
kind delete cluster
kind create cluster
argocd-autopilot repo bootstrap --recover
```

Full wipe

```
cd argocd-autopilot-test
git pull
kind delete cluster
rm -rf apps bootstrap project 
git add .
git commit -m "chore: full wipe"
git push
kind create cluster
argocd-autopilot repo bootstrap
git pull
```

