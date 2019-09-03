
#### Build hive image and deploy container 
```
1. git clone https://{user}@bitbucket.org/libertywireless/alluxio-presto.git
2. cd alluxio-presto/hive
3. docker build -t hive-image .
4. docker run -d --shm-size 1G --net=alluxio_net -p 9083:9083 --name=hive-00 hive-image
```

#### Start hive-shell
`docker exec -u datamaster -it hive-00 hive`

#### Create sample alluxio tables in hive(replace LOCATIONS with your IPs and paths)
* Text File:
`create external table random_num(identity int) STORED AS TEXTFILE LOCATION 'alluxio://zk@{zk1}:2181,{zk2}:2181,{zk3}:2181/directory_path_to_files';`

* CSV File:
`create external table movies(movieId INT, title string, genres string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LOCATION 'alluxio://zk@{zk1}:2181,{zk2}:2181,{zk3}:2181/directory_path_to_file';`

* Parquet File:
`create external table users_hive(id int, first_name string, last_name string, email string, gender string, ip_address string, cc string, country string, salary DOUBLE, title string, comments string) STORED AS PARQUET LOCATION 'alluxio://zk@{zk1}:2181,{zk2}:2181,{zk3}:2181/directory_path_to_file';`

#### Start hive metastore(thrift server)
`docker exec -u datamaster -it hive-00 hive --service metastore`

#### Troubleshooting
#### Get docker containers
```docker ps```
##### Login into the container
```docker exec -it {docker_id} /bin/bash```
##### Check container logs
```docker container logs --tail 10000 -f {docker_id}```
