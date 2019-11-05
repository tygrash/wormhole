
#### Build presto-worker image and deploy container 
```
1. git clone https://{user}@bitbucket.org/libertywireless/alluxio-presto.git
2. cd alluxio-presto/presto-worker
3. docker build -t presto-worker-image .
4. docker run -d -u datamaster --shm-size 3G -p 8080:8080 --net=wormhole_net --name=presto-worker-00 presto-worker-image
```

#### Troubleshooting
#### Get docker containers
```docker ps```
##### Login into the container
```docker exec -it {docker_id/docker_name} /bin/bash```
##### Check container logs
```docker container logs --tail 10000 -f {docker_id/docker_name}```
