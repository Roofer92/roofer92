# Harbor registry

## Development

### Push docker image to registry
````bash
docker build . -t <image-name>:<tag>
docker tag demo-app:1.0.0 <registry URL>/<project>/demo-app:1.1.0
docker push <registry URL>/<project>/demo-app:1.1.0
````