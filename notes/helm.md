# Helm

## Development

### Deploy demo-app with helm and use local image

build and tag image
```shell
docker build -f .\Dockerfile  -t local/demo-app:latest . 
```

with args
```shell
--build-arg VERSION="SNAPSHOT-X.Y.Z"
```

helm chart values.yaml config to use local image:
```yaml
image:
    repository: local/demo-app
    pullPolicy: Never
    tag: "latest"
```