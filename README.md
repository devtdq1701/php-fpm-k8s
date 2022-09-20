**Step 1**: Setup a NFS Server and configure your worker nodes to NFS clients

- Tutorial: https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-20-04

**Step 2**: Deploy NFS Subdir External Provisioner to your cluster

- Tutorial: https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner

**Step 3**: Installing metallb for LoadBalancer service

- Tutorial: https://metallb.universe.tf/installation/

To install MetalLB, apply the manifest:

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.5/config/manifests/metallb-native.yaml
```

Setup address pool used by loadbalancers (using pod cidr):

```
# metallb-config.yaml
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: metallb-config
  namespace: metallb-system
spec:
  addresses:
  - 192.168.255.200-192.168.255.250
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: empty
  namespace: metallb-system
```

**Step 4**: Installing nginx ingress controller

```bash
k label nodes <master_hostname> ingress-ready=true
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

**Step 5**: Test

Using `Curl`:

```
curl --resolve quangtd.com:80:<master_ip> http://quangtd.com/info.php -v
```

Add the following entry to your /etc/hosts file:

```
<master_ip> quangtd.com
```
