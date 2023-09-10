# Kubernetes

## Best Practices


### kube config

#### merge two config files

```
# Make a copy of your existing config 
$ cp ~/.kube/config ~/.kube/config.bak 
# Merge the two config files together into a new config file 
$ KUBECONFIG=~/.kube/config:/path/to/new/config kubectl config view --flatten > /tmp/config 
# Replace your old config with the new merged config 
$ mv /tmp/config ~/.kube/config 
# (optional) Delete the backup once you confirm everything worked ok 
$ rm ~/.kube/config.bak
```
<!--

## Useful Links

### Cheatsheets
- []()
- []()

### Articles
- []()
- []()

### Youtube
- []()
- []()

### Pluralsight/Lynda/LinkedIn Learning Courses
- []()
- []()

### Books to read
- []()
- []()

### Stackoverflow
- []()
- []()

-->