# k3sup

> All these commands are run from your computer, not the RPi.

```bash
# Install k3sup locally
cd ~/Downloads
curl -sLS https://get.k3sup.dev | sh
sudo cp k3sup /usr/local/bin/
k3sup --help

# Install k3s on master node
k3sup install --ip 10.128.128.23 \
    --k3s-version v1.17.3+k3s1 \
    --user hypobot \
    --k3s-extra-args '--no-deploy servicelb --no-deploy traefik --no-deploy metrics-server --default-local-storage-path /k3s-local-storage'

# Make kubeconfig accessable globally
mkdir ~/.kube
mv ./kubeconfig ~/.kube/config

# Join worker nodes into the cluster
k3sup join --ip 10.128.128.24 \
    --server-ip 10.128.128.23 \
    --k3s-version v1.17.3+k3s1 \
    --user hypobot

k3sup join --ip 10.128.128.25 \
    --server-ip 10.128.128.23 \
    --k3s-version v1.17.3+k3s1 \
    --user hypobot

# You should be able to see all your nodes
kubectl get nodes

# View config for the Cluster
kubectl get configmap coredns -n kube-system -o yaml
```
