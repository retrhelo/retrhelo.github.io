---
title: "SSH to container on remote server"
date: 2022-04-07T16:13:10+08:00
tags: ["docker"]
---

Containerization is a concept of importance in today's network service. And
_Docker_ is one of the most successful container management tools. This article
notes down how I deploy a container on remote, and access it via SSH.

<!--more-->

## Create docker container

It's simple to create a container based on common Linux distributions. To make
it simple and easy to describe, let's assume we're using `ubuntu:18.04` as the
image for container.

```
docker run -ti --network host --name my_ubuntu ubuntu:18.04 /bin/bash
```

This command will create a new container named `my_ubuntu`.

- `-ti` creates the container as interactive, which doesn't exit immediately.
- `--network host` sets container to host mode, sharing the same network
  interface with host OS. It's not the best solution but the simplest to create
  a container with usable network.
- `--name my_ubuntu` specifies a name for container. Otherwise a random name is
  is given.
- `ubuntu:18.04` specifies the image `ubuntu` with tag `18.04`. If tag not
  given, then `latest` is used.
- `/bin/bash` is the first command to run as init, with PID 1.

## Install and configure ssh server

The image `ubuntu:18.04` is minimized for containerization purpose. To be
able to _ssh_ to it, we should first install `openssh-server` package.

```shell
# sudo not required since running as root
apt-get install openssh-server
```

Generally, we use `systemctl` to start a service, or a more UNIX-like term,
"[daemon][1]". However, since the init process is not `systemd`, we can't simply
run `systemctl start sshd.service` to achieve our purpose. Use
`service ssh start` instead.

[1]: https://en.wikipedia.org/wiki/Daemon_(computing)

But things just aren't that easy. Before starting the daemon, some
configurations must be set.

- There's a potential port conflict between host machine and the container.
  Since they share the same network interface, and both have ssh daemon to run,
  We should assign a tcp port for container's sshd different from the host's.
- By default, ssh as root is not allowed.

The configuration can be done to `/etc/ssh/sshd_config`, where the configs
exist. Add the following lines to the file for custom configuration.

```shell
# Use port 1234 for sshd to listen
Port 1234
# Allow login as root
PermitRootLogin yes
```

After completing the configs, type `service ssh start` to begin the daemon.

## Login to container from local

As container's network configured as host mode, host's IP would also be
container's IP. But by specifying the port, ssh will differ between login into
host and the container.

Logging into the container is no magic, but a little tricks: remember to assign
the port when use ssh. In our example, use `ssh -p 1234 root@host-ip`.
