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


### To add docker hub OCI registry as repository in ArgoCD
*replace username/password with Docker hub credential

```
apiVersion: v1
kind: Secret
metadata:
  name: oci
  namespace: argo-cd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  enableOCI: true
  name: docker
  project: default
  type: helm
  url: registry-1.docker.io
  username: <>
  password: <>
type: Opaque
```

#### Pushing helm chart to docker hub:
```
For example:
{
  helm package .
  helm registry login registry-1.docker.io -u <username>
  helm push cert-manager-v1.15.0.tgz oci://registry-1.docker.io/<username>
}
```
