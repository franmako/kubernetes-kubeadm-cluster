- Initialize your control-plane node (https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#initializing-your-control-plane-node) - KEEP OUTPUT FOR JOIN

```
#Specify pod network cidr for flannel
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```
- Deploy a Pod network to the cluster (flannel: https://github.com/flannel-io/flannel)

```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
- Make kubectl work for your non-root user

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
