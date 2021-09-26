- Join cluster
```
sudo kubeadm join <control-plane IP>:6443 --token <token> \
        --discovery-token-ca-cert-hash <hash>
```
