---
tags:
  - tool
---
# docker

Containerize applications

## Capabilities

```bash
# List all docker containers
sudo docker ps -a

# List boot configuration for all containers
sudo docker ps -a --format '{{.Names}}' | while read container; do echo -n "$container: "; sudo docker inspect -f '{{ .HostConfig.RestartPolicy.Name }}' "$container"; done

# Configure container to not start on boot
docker update --restart=no $CONTAINER
```
