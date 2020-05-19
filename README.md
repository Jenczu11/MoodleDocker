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

Create directory for mariadb `mkdir mariadb-data && sudo chmod o+w mariadb-data`

Make sure your account is owner NOT root! 
If not run
```bash
sudo chown -R user:user {directory}
```
Starting containers
```bash
docker-compose up
```
## Adminer
Adminer is accesible on port 8080

Data to login by default is
```
Server : moodledocker_mariadb_1
user: bn_moodle
password: zaq12wsx
```

To check on what mariadb ip is you have to run commands when containers are running
```bash
docker network ls 
```
Find network with proper name (eg. moodle_docker_default in bridge mode), 

Next with network name or it's id (you can only write first 2 letters if unique)
```bash
docker network inspect moodledocker_default
```
And look for data where name is like `moodledocker_mariadb_1`
```json
   "adb05a318dc30560f79548bb93aff7f850719c3ec4cc7fba5a400eb1e5f12642": {
                "Name": "moodledocker_mariadb_1",
                "EndpointID": "24f324a2e97fa4654753737a87c3f9d9b70821740cb51671dcb67dcf4310c293",
                "MacAddress": "02:42:ac:15:00:03",
                "IPv4Address": "172.21.0.3/16",
                "IPv6Address": ""
            }
```
Now with that data you can login with `"Name"` or `"IPv4Address"`

## MariaDB

MariaDB is on port 3306

## Moodle platform

Moodle is on port **80** and you can access it by typing localhost:80 or simply localhost.

## Clean up

To clean up and be able to start fresh run where docker-compose is
Removes all not used containers, cleans up all unused volumes and removes mdl-data folder with moodle data.
```bash
docker container prune && docker volume prune && sudo rm -rf mdl-data
```

# How to move data between hosts in WSL
**Neccessary!!! Have on both WSL instances same user (with the same username; not tested otherwise).**

Then in parent directory of git clone (where you have mdl-data, mariadb-data and docker-compose.yml).

**Run command** 
```bash
sudo tar -pcvzf mdl.tar.gz MoodleDocker/
```
* p == preserve permissions
* c == create archive
* v == verbose (print name of files)
* z == gzip (to save some space other ~80Mb instead of 500Mb)
* f == tar file name (in this case `mdl.tar.gz`)
Then move file to another machine in any way you want. 

**Now you can restore whole directory with command**
```bash
sudo tar -pxvzf mdl.tar.gz MoodleDocker/
```
**Sudo is needed to restore proper permissions** on every directory and file (that's why you should do it on same user)