
#### Build alluxio-master image and deploy container 
```
1. git clone https://{user}@bitbucket.org/libertywireless/alluxio-presto.git
2. cd alluxio-presto/alluxio-master
3. docker build -t alluxio-master-image .
4. docker run -d --shm-size 3G -u 12345 -p 19999:19999 --net=wormhole_net --name=alluxio-master-00 alluxio-master-image master
```

#### Troubleshooting
#### Get docker containers
```docker ps```
##### Login into the container
```docker exec -it {docker_id/docker_name} /bin/bash```
##### Check container logs
```docker container logs --tail 10000 -f {docker_id/docker_name}```
