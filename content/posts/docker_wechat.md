---
title: "Set up WeChat on Linux with Docker"
date: 2022-06-18T23:12:49+08:00
tags: ["docker"]
draft: false
---

WeChat is a popular IM (Instant Message) software in China, and is widely used
in business situations. However, Tecent, the corporation behind WeChat, serves
to provide no support for this popular app in Linux environment.

<!--more-->

In this article, I introduce how I set up my WeChat environment on my Fedora 36,
with the help of docker and wine.

## Docker setup

How I set docker environment up on my machine is not part of the article.
Related contents can be easily found on the Internet, thus not mentions here.

## Find the docker image

There's already existing docker image that serves to provide WeChat on docker.
It's [_DoChat_][1] provided by Huan. The author provides an easy script for the
whole installation, which I browsed and then wrote this article.

[1]: https://github.com/huan/docker-wechat

The docker image to use is `zixia/wechat`. To pull it down from remote, use

```bash
docker pull zixia/wechat:latest
```

It took a while to get everything ready, depending on the network.

## Create docker container

Container is the instance of certain image. To create a container, use
`docker run`. Here, to create my docker image, I use

```bash
docker run \
    -i \
    --name wechat \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v /home/retrhelo/Downloads/wechat:'home/user/WeChat Files' \
    -e DISPLAY \
    -e DOCHAT_DEBUG \
    -e DOCHAT_DPI \
    -e XMODIFIERS \
    -e GTK_IM_MODULE \
    -e QT_IM_MODULE \
    --ipc=host \
    --privileged \
    zixia/wechat:latest
```

There are lots of details to be told in the above command. Here I introduce
how we create a file-sharing fold to transfer files between the host and the
container.

### Set up a sharing folder

You may notice that I use the argument `-v` frequently -- This flag is the key
to create a sharing folder. In the official document, `-v` is used to
_bind mount a volume_, which preserves a folder to be shared between the host
and container.

By using this, I set the container to share `/home/user/WeChat Files` folder
with the host, where WeChat stores the files received.

### Enable input method

Some environments should be set when creating the container, for the input
method to work properly. They're `XMODIFIERS`, `GTK_IM_MODULE` and
`QT_IM_MODULE`. Without setting them, the method won't work when typing into
the container.

## Run the container

After setting up, start the container with `docker start -i wechat`. It would
trigger the following window to appear.

<div align="center">
<img src="/images/wechat-start.png" width="30%", height="30%"/>
</div>

If it's the first log in, then a QR code would be displayed. Use your mobile
WeChat app to scan the code, just like the windows version of WeChat.

After the login, WeChat is ready for use! But be noticed that this isn't always
stable, and crashes sometimes. Use docker to restart when it crashes.
