### ArgoCD Installation

https://artifacthub.io/packages/helm/argo/argo-cd

```
{
    https://github.com/argoproj/argo-helm/releases/download/argo-cd-7.1.3/argo-cd-7.1.3.tgz
    tar xzvf argo-cd-7.1.3/argo-cd-7.1.3.tgz
    cd argo-cd
}
```

```
helm -n argo-cd install argo-cd --values values.yaml --values https://raw.githubusercontent.com/kaviankarimzadeh/argocd-apps/main/values/argo-cd-values.yaml . --create-namespace
```

