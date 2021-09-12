# Docker registry

![License](https://img.shields.io/packagist/l/cakephp/app.svg?style=flat-square)

## Features

- Simple, Small, Secure docker registery
- Proxy docker, gcr, quay, k8s
- Use secure transportation by helping HTTPS
- Memeory usage < 35mb and very small computation resource

## Installations

### Clone Source:

```bash
$ git clone https://github.com/mimani68/registry-mirror.git
```
and start it up:

```bash
$ docker-compose up
```
It's done.


## Client setup

Modify or create the file `/etc/docker/daemon.json` and add the local mirror setting pointing to your server on port 5000:

```json
{
  "registry-mirrors": ["https://hub.dckr.ir"]
}
```

Then restart docker daemon:

```bash
$ service docker stop
$ service docker sart
```

and finally you can download files using below command.

```bash
$ docker pull apline:latest
```
