# Kubernetes

## Best Practices

### kube config

#### merge two config files

Power Shell
https://www.mrjamiebowman.com/software-development/kubernetes/merge-kube-config-in-powershell/

```shell
# make a backup
cd ~/.kube/
cp config config.bak
 
# merge both kube config files
$ENV:KUBECONFIG = "C:\Users\roofer92\.kube\config;C:\Users\roofer92\.kube\config-devbox"
 
# verify that the variable is set
$ENV:KUBECONFIG
 
# output to temp file
kubectl config view --flatten > config-merged
 
# verify that config-merged is correct
kubectl --kubeconfig=config-merged config get-clusters
 
# delete backup
rm config
 
# move merged file to config
mv config-merged config
 
# remove (optional)
rm config.bak
```

Bash
```shell
# Make a copy of your existing config 
$ cp ~/.kube/config ~/.kube/config.bak 
# Merge the two config files together into a new config file 
$ export KUBECONFIG=~/.kube/config:/path/to/new/config 
$ kubectl config view --flatten > /tmp/config 
# Replace your old config with the new merged config 
$ mv /tmp/config ~/.kube/config 
# (optional) Delete the backup once you confirm everything worked ok 
$ rm ~/.kube/config.bak
```

### Job

#### Start suspended Job

```
$ kubectl patch job/job-name --type=strategic --patch '{\"spec\":{\"suspend\":false}}'
```

### Pull an Image from a private Registry
````
kubectl create secret docker-registry regcred --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword>
````

### Secret

Create Credentials to pull from private registry
````
kubectl create secret docker-registry regcred --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
````

## Debugging

### Debug pod

Apply a Pod for debug purpose of the image

````yaml
apiVersion: v1
kind: Pod
metadata:
  name: debug-pod
  namespace: default
spec:
  containers:
    - name: debug-container
      image: <your-image-name>
      command: [ "/bin/sleep" ]  # Sleep command to keep the container running
      args: [ "infinity" ]  # Sleep indefinitely to prevent the container from exiting
````

Alternatively the ```sleep infinity``` command can be added to a deployments image, to keep a container running.



## Monitoring

### Show more Datapoints
The default metrics show datapoints in 2m intervals. To show more datapoints open Grafana, [login as admin](https://ranchermanager.docs.rancher.com/how-to-guides/advanced-user-guides/monitoring-alerting-guides/customize-grafana-dashboard) 
and lower the min Interval for the query under the Panels query options.

### Edit Scrape Config for labeled pods
The global scrape_interval is 1 minute. To scrape metrics for pods with a custom label add the following job to prometheus scrape_configs:

```yaml
  - job_name: 'kubernetes-pods-300s'
 
    scrape_interval: 300s
 
    kubernetes_sd_configs:
    - role: pod
 
    # example relabel to scrape only pods that have 'example.io/should_be_scraped_every_300s: "true"' annotation
    relabel_configs:
    - source_labels: [__meta_kubernetes_pod_annotation_example_io_should_be_scraped_every_300s]
      action: keep
      regex: true
 
    # rest of this config was taken from default 'kubernetes-pods' job already present in default Prometheus config
    - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
      action: keep
      regex: true
    - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
      action: replace
      target_label: __metrics_path__
      regex: (.+)
    - source_labels: [__address__, __meta_kubernetes_pod_annotation_prometheus_io_port]
      action: replace
      regex: ([^:]+)(?::\d+)?;(\d+)
      replacement: $1:$2
      target_label: __address__
    - action: labelmap
      regex: __meta_kubernetes_pod_label_(.+)
    - source_labels: [__meta_kubernetes_namespace]
      action: replace
      target_label: kubernetes_namespace
    - source_labels: [__meta_kubernetes_pod_name]
      action: replace
      target_label: kubernetes_pod_name
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