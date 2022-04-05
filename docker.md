# Docker snippet cookbook

## table of contents

- [attach a container to a running container](#attach-a-container-to-a-running-container-network)
- [subnet collisions](#subnet-collisions)

## snippets

### Attach a container to a running container network

```sh
docker run --name web -p 9999:80 -d nginx  # a container running
docker run -it --rm --net container:web nicolaka/netshoot ss
```


### Subnet collisions

```/etc/docker/daemon.json
{
  "bip": "10.15.0.1/24"
}
```
