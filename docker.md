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