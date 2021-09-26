- Install helm
```
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

- Install a load balancer (metalLB: https://metallb.universe.tf/installation/#installation-by-manifest)

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.10.2/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.10.2/manifests/metallb.yaml
kubectf apply -f metallb/metallb-config.yaml
```
- Install nginx ingress controller (https://kubernetes.github.io/ingress-nginx/deploy/)

````
#Add helm repo
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

#Install helm chart
helm install ingress-nginx ingress-nginx/ingress-nginx -n ingress-nginx --create-namespace
````
- Install cert manager (https://cert-manager.io/docs/installation/helm/)

````
#Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io

#Install with custom resource definitions
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.5.3 \
  --set installCRDs=true

#Add let's encrypt certificate issuer
kubectl apply -f cert-manager/cluster-issuer-le-prod.yaml
````
- Install longhorn (https://longhorn.io/docs/1.2.0/deploy/install/install-with-helm/)

```sh
#Install jq
sudo apt install -y jq

#install open-iscsi on all nodes
sudo apt-get -y install open-iscsi

#Check environment requirements on control-plane node
curl -sSfL https://raw.githubusercontent.com/longhorn/longhorn/v1.2.0/scripts/environment_check.sh | bash

#install with helm
helm install longhorn longhorn/longhorn --namespace longhorn-system --create-namespace

#upgrade with helm
#helm upgrade -n longhorn-system longhorn longhorn/longhorn

#Generate basic auth password
htpasswd -c auth john

#create basic auth secret from file
kubectl create secret generic basic-auth --from-file=./longhorn/auth -n longhorn-system

#Add ingress
kubectl apply -f longhorn/ingress.yaml

```
