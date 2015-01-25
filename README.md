**This github repository will not be seeing any further updates for the time being. The reason is that the associated Docker repository is bugged for some time (I cannot link because the Docker Support tracker is not public...) and the bug has not been resolved. In addition, the versioning is wrong here. The main request has been to track versions automatically from sinopia upstream. This repo is not designed properly for such a case. Perhaps in the future I will script this, when I setup sinopia again.**

**If you depend on this repo, please accept my apologies. My recommendation to you is to search "sinopia" on the Docker registry and find one of the newer repositories that has is not affected by these issues -- or you can fork this repo and push to a fresh Docker repository that does not have the bug in question. Sorry for the inconvenience.**

## Sinopia (Docker Image)

Sinopia is a private npm repository server

### Installing Image

`docker pull keyvanfatehi/sinopia:0.13.0`

### Creating Container

`docker run --name sinopia -d -p 4873:4873 keyvanfatehi/sinopia:0.13.0`

### Setting Registry

`npm set registry http://<docker_host>:4873/`

### Determining Username and Password

`docker logs sinopia`

### Modify configuration

There are two ways to modify the configuration.

To understand the difference, view the conversation here: https://github.com/keyvanfatehi/docker-sinopia/pull/10

### Original Method

```
docker stop sinopia
docker run --volumes-from sinopia -it --rm ubuntu vi /opt/sinopia/config.yaml
docker start sinopia
```

### Alternative Method

```
# Save the config file
curl -L https://github.com/rlidwka/sinopia/blob/master/conf/default.yaml -o /path/to/config.yaml
# Mount the config file to the exposed data volume
docker run -v /path/to/config.yaml:/opt/sinopia/config.yaml --name sinopia -d -p 4873:4873 keyvanfatehi/sinopia:0.13.0
```

Restart the container anytime you change the config.

### Backups

`docker run --volumes-from sinopia -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /opt/sinopia`

Alternatively, host path for /opt/sinopia can be determined by running:

`docker inspect sinopia`

### Restore

```
docker stop sinopia
docker rm sinopia
docker run --name sinopia -d -p 4873:4873 keyvanfatehi/sinopia:0.12.0
docker stop sinopia
docker run --volumes-from sinopia -v $(pwd):/backup ubuntu tar xvf /backup/backup.tar
docker start sinopia
```

## Links

* [Sinopia on Github](https://github.com/rlidwka/sinopia)
