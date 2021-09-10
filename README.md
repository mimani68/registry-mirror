# Docker registry cache

![License](https://img.shields.io/packagist/l/cakephp/app.svg?style=flat-square)

## Concept

- Create a Docker registry cache for local network to speed up docker image download and keep a lower bandwidth.
- Use docker-compose.

## Cache (server) setup

### Clone this repo:

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
  "registry-mirrors": ["http://<my-docker-mirror-host-ip>:5000"]
}
```

Then restart docker daemon:

```bash
$ service docker stop
$ service docker sart
```

## Test
Verify that everything is working correctly running a new image on the client while watching server's log. First check if the required image is locally available,

```bash
$ docker image ls -a
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
...
hello-world                   latest              48b5124b2768        14 months ago       1.84kB
...
```
and remove it (hello-world in this case) if exists:

```bash
$ docker image rm 48b5124b2768
```
Verify your registry cache is empty. From the client query the server trhough the exposed API:

```bash
$ curl http://<my-docker-mirror-host-ip>:5000/v2/_catalog
# outputs -> {"repositories":[]}
```

In the client, run hello-world:

```bash
$ docker run hello-world
```
and verify it is present on your registry cache (server):

```bash
$ curl http://<my-docker-mirror-host-ip>:5000/v2/_catalog
# outputs -> {"repositories":["library/busybox"]}
```

In the server log some line should pass showing desired activity too.
The images downloaded will be stored in the server `data` folder and listed in the nested folder `data/docker/registry/v2/repositories/library/`.


