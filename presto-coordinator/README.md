
#### Build presto-coordinator image and deploy container 
```
1. git clone https://{user}@bitbucket.org/libertywireless/alluxio-presto.git
2. cd alluxio-presto/presto-coordinator
3. docker build -t presto-coordinator-image .
4. docker run -d --shm-size 3G -u datamaster -p 8080:8080 --net=alluxio_net --name=presto-coordinator presto-coordinator-image
```

#### Troubleshooting
#### Get docker containers
```docker ps```
##### Login into the container
```docker exec -it {docker_id} /bin/bash```
##### Check container logs
```docker container logs --tail 10000 -f {docker_id}```
