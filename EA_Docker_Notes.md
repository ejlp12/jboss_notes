Login to existing running container:

`sudo docker exec -i -t <docker_id> /bin/bash`

```
docker login -u ejlp12 registry.hub.docker.com
docker tag tomcat ejlp12/tomcat
docker push ejlp12/tomcat
```

Clean up (be careful):
```
docker rm $(docker ps -q -f 'status=exited')
docker rmi $(docker images -q -f "dangling=true")
```
