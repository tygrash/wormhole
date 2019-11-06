
#### Build alluxio-master image and deploy container 
```
1. https://github.com/{github_user}/wormhole.git
2. cd wormhole
3. docker build -t alluxio-master-image -f alluxio-master/Dockerfile .
4. docker run -d --shm-size 3G -u 12345 -p 19999:19999 --net=wormhole_net --name=alluxio-master-00 alluxio-master-image master
```

#### Troubleshooting
#### Get docker containers
```docker ps```
##### Login into the container
```docker exec -it {docker_id/docker_name} /bin/bash```
##### Check container logs
```docker container logs --tail 10000 -f {docker_id/docker_name}```
