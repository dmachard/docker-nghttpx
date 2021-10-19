# Nghttpx Docker Image

This *[nghttpx](https://github.com/nghttp2/nghttp2/) Docker Image* is based on **Debian**.

## Supported tags and respective `Dockerfile` links

- [`1.45.1-r0`, `latest`](https://github.com/dmachard/nghttpx-docker/tree/main/1.45.1)

## How to use this image

Run this container with the following command:

```
sudo docker run --name nghttpx01 -d -p 3000:3000/tcp --restart=always dmachard/nghttpx:latest
```

## Custom configuration

You can run this image and provide your own nghttpx configuration like that:

```
sudo docker run --name nghttpx01 -d -p 3000:3000/tcp -v $PWD/mynghttpx.conf:/opt/nghttp2/etc/nghttpx.conf \
dmachard/nghttpx:latest
```