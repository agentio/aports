# Agent IO aports repository

This repository contains the APKBUILD files for Alpine Linux packages that are maintained for the Agent IO project, along with the required patches and scripts, if any.

## Repositories

This aports tree consists of a single repository (directory) called "agent.io" that contains packages published by Agent IO.

## Building

The entire repository can be built with the `buildrepo` tool.

## Docker caveat

When running an alpine image in Docker, use the following option to avoid problems with `fakeroot`:

```
--ulimit nofile=1024:524288
```

## Raw notes

```
docker run -ti --name alpine --ulimit nofile=1024:524288 alpine:latest
docker restart alpine
docker exec -ti alpine sh
```

In the container as `root`:
```
apk add alpine-sdk lua-aports
adduser agent-io -D
addgroup agent-io abuild
apk add go protoc
```

as `agent-io`
```
abuild-keygen -a
git clone https://github.com/agentio/aports
```

as `root`
```
cp /home/agent-io/.abuild/*.rsa.pub /etc/apk/keys/
```

as `agent-io`
```
PATH=/home/agent-io/go/bin:$PATH

buildrepo agent.io
```
