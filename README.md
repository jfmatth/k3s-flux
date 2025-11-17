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
