# Moodle Docker
Moodle Docker setup based on bitnami setup

Tested on WSL 2 setup with docker (docker-compose running on WSL with linux containers)

```
winver.exe 2004
Docker version 19.03.8, build afacb8b7f0
```

Don't use on <b>production</b> without proper security changes only on development.

## Main sites if any problems
https://hub.docker.com/r/bitnami/moodle/

https://github.com/bitnami/bitnami-docker-mariadb/issues/193

https://github.com/bitnami/bitnami-docker-moodle/issues/69

## Description

First change all passwords in docker-compose file (it is 'xxxx' -> to any password)

### Adminer
Adminer is accesible on port 8080

Data to login
```
Server : mariadb
user: root
password: xxxx
```

### MariaDB

MariaDB is on port 3306

### Moodle

Moodle is on port <b>80</b> -> localhost:80 or simply localhost.

### Clean up

To clean up and be able to start fresh run where docker-compose is
Removes all not used containers, cleans up all unused volumes and removes mdl-data folder with moodle data.
```bash
docker container prune && docker volume prune && sudo rm -rf mdl-data
```

