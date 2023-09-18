# MySQL 8 Docker, with primary and replica 


## How To use this project, a little prep work is needed. 
```Go
Working with Docker creds:

Create a file called .env in the root of the project and add the following:
MYSQL_ROOT_PASSWORD=s3cr3t

```

## Replica password
```bash
The replica password used for testing is in the replica.sql file. I would recommend changing this password with an alter user command.
An example of this is in the primary.sql file.
```

## To build the primary section, you can run the following command:
```Go

docker build -t mysql-primary .

``` 

## To build the replica section, you can run the following command:
    
```Go
docker build --build-arg build_type=replica -t mysql-replica .

```

## docker-compose

```Go
docker-compose up -d --wait



docker-compose up -d --wait

[+] Running 3/3
 ✔ Network mysql8-docker_db-network         Created                                                                                                                                                                               0.1s
 ✔ Container mysql8-docker-mysql-primary-1  Healthy                                                                                                                                                                               0.1s
 ✔ Container mysql8-docker-mysql-replica-1  Healthy                                                                                                                                                                               0.0s

```

## Interact with the primary database

```bash
mysql --defaults-group-suffix=_primary1 -e "select @@read_only"
mysql --defaults-group-suffix=_replica1 -e "select @@read_only" 

mysql --defaults-group-suffix=_replica1 -e "show replica status\G" 
```

## Interact with the replica database

```bash
mysql8-docker on  main [!?] via 🐳 desktop-linux 
❯ mysql --defaults-group-suffix=_primary1 -e "select @@read_only"
+-------------+
| @@read_only |
+-------------+
|           0 |
+-------------+

mysql8-docker on  main [!?] via 🐳 desktop-linux 
❯ mysql --defaults-group-suffix=_replica1 -e "select @@read_only"
+-------------+
| @@read_only |
+-------------+
|           1 |
+-------------+

mysql8-docker on  main [!?] via 🐳 desktop-linux 
❯ mysql --defaults-group-suffix=_replica1 -e "select @@server_id"
+-------------+
| @@server_id |
+-------------+
|           2 |
+-------------+

mysql8-docker on  main [!?] via 🐳 desktop-linux 
❯ mysql --defaults-group-suffix=_primary1 -e "select @@server_id"
+-------------+
| @@server_id |
+-------------+
|           1 |
+-------------+


mysql8-docker on  main [!?] via 🐳 desktop-linux 
❯ mysql --defaults-group-suffix=_replica1 -e "show replica status\G" | egrep "Replica_IO_Running:|Replica_SQL_Running:|Seconds_Behind_Source:|Source_Host:" 
                  Source_Host: 172.20.0.3
           Replica_IO_Running: Yes
          Replica_SQL_Running: Yes
        Seconds_Behind_Source: 0
```
