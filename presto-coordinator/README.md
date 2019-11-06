
#### Build presto-coordinator image and deploy container 
```
1. https://github.com/{github_user}/wormhole.git
2. cd wormhole
3. docker build -t presto-coordinator-image -f presto-coordinator/Dockerfile .
4. docker run -d --shm-size 3G -u datamaster -p 8080:8080 --net=wormhole_net --name=presto-coordinator presto-coordinator-image
```

#### Troubleshooting
#### Get docker containers
```docker ps```
##### Login into the container
```docker exec -it {docker_id/docker_name} /bin/bash```
##### Check container logs
```docker container logs --tail 10000 -f {docker_id/docker_name}```
