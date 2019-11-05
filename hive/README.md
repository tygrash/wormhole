
#### Build hive image and deploy container 
```
1. git clone https://{user}@bitbucket.org/libertywireless/alluxio-presto.git
2. cd alluxio-presto/hive
3. docker build -t hive-image .
4. docker logs mysql1 2>&1 | grep GENERATED
5. docker exec -it mysql1 mysql -uroot -p'{password_obtained_from_above_command_4.}'
   5.1. ALTER USER 'root'@'localhost' IDENTIFIED BY '{password_for_root}';
   5.2. CREATE database metastore;
   5.3. CREATE USER 'hiveuser'@'%' IDENTIFIED BY 'hivepassword';
   5.4. GRANT ALL PRIVILEGES ON metastore . * TO 'hiveuser'@'%'; 
   5.5. SOURCE /opt/apache-hive-2.1.0-bin/scripts/metastore/upgrade/mysql/hive-schema-0.14.0.mysql.sql;
4. docker run -d --shm-size 1G --net=wormhole_net -p 9083:9083 --name=hive-00 hive-image
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

* JSON File:
```
 set hive.support.sql11.reserved.keywords=false;

 create external table clickstream(activity string, timestamp bigint, userDetails string) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe' LOCATION 'alluxio://zk@{zk1}:2181,{zk2}:2181,{zk3}:2181/directory_path_to_file';
 
 ALTER TABLE clickstream SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");
```

#### Start hive metastore(thrift server)
`docker exec -u datamaster -it hive-00 hive --service metastore > metastore.log 2>&1 &`

#### Troubleshooting
#### Get docker containers
```docker ps```
##### Login into the container
```docker exec -it {docker_id/docker_name} /bin/bash```
##### Check container logs
```docker container logs --tail 10000 -f {docker_id/docker_name}```
