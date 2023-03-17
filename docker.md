# Docker snippet cookbook

## table of contents

- [attach a container to a running container](#attach-a-container-to-a-running-container-network)
- [subnet collisions](#subnet-collisions)
- [enable internet on bridge network](#enable-internet-on-bridge-network)
- [run a command inside a container from a packer-built image](#packer-moment)
- [inspect - show image layers](#inspect-ls-layers)
- [History-of-changes-layer-by-layer](#History-of-changes-layer-by-layer)

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

### inspect a container and filter the result for a value

```sh
docker inspect --format='{{json .State.Health.Status}}' selenium-hub
```

### enable internet on bridge network

https://docs.docker.com/network/bridge/#enable-forwarding-from-docker-containers-to-the-outside-world

* these do not persist after reboot

```sh
sudo sysctl net.ipv4.conf.all.forwarding=1
sudo iptables -P FORWARD ACCEPT
```

### Packer moment

```sh
packer build packer-config.hcl  # creates a docker image
docker run -it --rm the-created-image /bin/sh  # /bin/sh: 1: Syntax error: "(" unexpected
docker run -it --rm the-created-image -c "/bin/sh"  # works fine
```

### Inspect ls layers

```
docker image inspect --format '{{json .RootFS.Layers}}' <image-id> | jq .
```

### History of changes layer by layer

* layers sorted from newest to oldest

```sh
docker image history 426d9fce83f2 --format 'table{{.ID}}\t{{.CreatedSince}}\t{{.CreatedBy}}\t{{.Size}}' | less
```
