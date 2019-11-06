
#### Build presto-worker image and deploy container 
```
1. https://github.com/{github_user}/wormhole.git
2. cd wormhole
3. docker build -t presto-worker-image -f presto-worker/Dockerfile .
4. docker run -d -u datamaster --shm-size 3G -p 8080:8080 --net=wormhole_net --name=presto-worker-00 presto-worker-image
```

#### Troubleshooting
#### Get docker containers
```docker ps```
##### Login into the container
```docker exec -it {docker_id/docker_name} /bin/bash```
##### Check container logs
```docker container logs --tail 10000 -f {docker_id/docker_name}```
