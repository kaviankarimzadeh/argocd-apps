### ArgoCD Installation

https://artifacthub.io/packages/helm/argo/argo-cd

```
{
    wget https://github.com/argoproj/argo-helm/releases/download/argo-cd-8.1.1/argo-cd-8.1.1.tgz
    tar xzvf argo-cd-8.1.1.tgz
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

#### to add local cluster
```
argocd cluster add kubernetes-admin@cluster.local --name cloud1
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


#### rook-ceph clean-up

```
https://rook.io/docs/rook/latest/Getting-Started/ceph-teardown/#delete-the-data-on-hosts


sudo sgdisk --zap-all /dev/sdb
sudo dd if=/dev/zero of=/dev/sdb bs=1M count=100 oflag=direct,dsync
sudo blkdiscard /dev/sdb
sudo partprobe /dev/sdb

sudo rm -rf /dev/ceph-*
sudo rm -rf /dev/mapper/ceph--*
sudo rm -rf /var/lib/rook/
```