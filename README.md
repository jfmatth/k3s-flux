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

### Datadog
https://us5.datadoghq.com/fleet/install-agent/latest?platform=kubernetes

Install operator with secret from DD

Create agent with ```datadog-agent.yaml```
```
kubectl apply -f datadog-agent.yaml -n datadog
```

## Install Flux (https://fluxcd.io/flux/guides/image-update/)
FINE GRAINED Access Token is setup in GH

### Setup variables
```
$env:GITHUB_TOKEN=" "

```
### Check and install
```
flux check --pre
```

```
flux bootstrap github `
  --components-extra=image-reflector-controller,image-automation-controller `
  --owner=jfmatth `
  --repository=k3s-flux `
  --token-auth `
  --branch=master `
  --private=false `
  --personal `
  --path=./clusters/my-cluster
```

**Make sure to use ```--token-auth``` for https only, otherwise it will default to SSH**


<!-- ### Podinfo 
Following the Getting Started, add PodInfo to Flux
```
flux create source git podinfo `
  --url=https://github.com/stefanprodan/podinfo `
  --branch=master `
  --interval=1m `
  --export > ./clusters/my-cluster/podinfo-source.yaml
```

```
flux create kustomization podinfo `
  --target-namespace=default `
  --source=podinfo `
  --path="./kustomize" `
  --prune=true `
  --wait=true `
  --interval=30m `
  --retry-interval=2m `
  --health-check-timeout=3m `
  --export > ./clusters/my-cluster/podinfo-kustomization.yaml
```

### JustNeverLift
```
flux create image repository justneverlift `
--image=ghcr.io/jfmatth/justneverlift.com `
--interval=5m `
--export > .\clusters\my-cluster\justneverlift.com\jnl-imagerepository.yaml
```

```
flux create image policy justneverlift `
--image-ref=justneverlift `
--select-semver=2025.0.0 `
--export > .\clusters\my-cluster\justneverlift.com\jnl-imagepolicy.yaml
``` -->