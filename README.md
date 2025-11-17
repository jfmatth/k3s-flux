# k3s-flux

## Install K3s via KS3sup

```
./k3sup install --host vps.3756home.org --user ubuntu
```

### Make sure UFW has the right ports open for K3s
```
ssh ubuntu@vps.3756home.org
sudo ufw allow 6443
```

### Setup ```KUBECONFIG```
Copy this kubeconfig from saved (non repo) location
```
$env:KUBECONFIG=".\kubeconfig"
```

### Validate
```
kubectl get nodes
```

## Install Flux (https://fluxcd.io/flux/get-started/)
Access Token is setup in GH

### Setup variables
```
$env:GITHUB_TOKEN=" "
$env:GITHUB_USER="jfmatth"
```
### Check and install
```
flux check --pre
```

```
flux bootstrap github `
  --owner=$env:GITHUB_USER `
  --repository=k3s-flux `
  --branch=master `
  --path=./clusters/my-cluster `
  --personal
```