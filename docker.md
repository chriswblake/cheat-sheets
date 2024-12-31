Log in to a container
```bash
sudo docker exec -it mycontainer bash
sudo docker exec -it mycontainer /bin/bash
```

Stop/Stop/Status
```bash
sudo docker container start mycontainer
sudo docker container stop mycontainer
sudo docker restart mycontainer
sudo docker inspect mycontainer
```

Pull and specify architecture
```bash
sudo docker pull --platform linux/amd64 mycontainer
```

Remove
```bash
sudo docker contaienr ls
sudo docker container rm mycontainer

sudo docker image ls
sudo docker image rm myimagehash
```

Publish
```bash
sudo docker login
sudo docker build -t ghcr.io/org-name/mycontainer:1.0.5 --build-arg USER=user --build-arg PASS=token .
```

Logs
```bash
sudo docker logs --tail 10 mycontainer
```

Compose
```bash
sudo docker-compose down
sudo docker-compose up -d
```

Tricks
```bash
# Show mounted volumes
sudo docker inspect -f '{{ .Mounts }}' edge-core-techconsole
```

Troubleshooting
```bash
# Error: error while loading shared libraries: libz.so.1: failed to map segment from shared object
sudo mount /tmp -o remount,exec
```


Docker Contexts
https://docs.docker.com/engine/reference/commandline/context/

```bash
# Create a context pointed at a remote server
docker context create remote-host --docker host=tcp:///my-remote-host:2735,key=key-file
```

```bash
# Change host URL
docker context update --docker "host=tcp://192.168.0.0,key=key-file" remote-host
```
> Note: `key-file` is the name of the file located in the `~/.ssh/` folder.

```bash
# Change context description
docker context update --description "description of context" remote-host
```