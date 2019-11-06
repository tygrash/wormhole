## Setup Zookeeper

```
1. docker pull zookeeper:3.5
2. docker run --name zookeeper-01 --restart always -p 2181:2181 -d zookeeper:3.5
```

#### Troubleshooting
#### Get docker containers
```docker ps```
##### Login into the container
```docker exec -it {docker_id/docker_name} /bin/bash```
##### Check container logs
```docker container logs --tail 10000 -f {docker_id/docker_name}```
