# WSL

## Best Practices

### Kubernetes

#### Link kube config to host version

```
# Mount kubeconfig from wsl to host:
# Start WSL2 session (e.g. Ubuntu) and add into ~/.profile:
export KUBECONFIG=/mnt/c/users/$USER/.kube/config
```

## Useful Links

### Stackoverflow
- [WSL2 - VPN DNS Workaround](https://superuser.com/questions/1582623/why-is-there-no-network-connectivity-in-ubuntu-using-wsl-2-behind-vpn)
