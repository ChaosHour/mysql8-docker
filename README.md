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

## To build and start the containers
```bash
make start
```

## To stop the containers and remove the containers and volumes.
```bash
make stop
make clean
```

## Using the ~/.my.cnf
```bash
[client_primary1]
user=root
password=xxxx
host=192.168.50.50
port=3306
[client_replica1]
user=root
password=xxxx
host=192.168.50.50
port=3307
```


## Interacting with the primary and replica

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
