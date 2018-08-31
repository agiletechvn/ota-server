# docker deployment code-push-server

>this document is used to guide how to deploy code-push-server on docker, including three parts:

- code-push-server
  - The default storage is `local`, but can change the volume storage method. Container destruction does not result in data loss unless manually deleted.
  - Internally use the pm2 cluster mode to manage processes. The default number of open processes is available cpu，but you can set the deploy parameter in the docker-compose.yml file.
  - Most of mandatory options are in docker-compose.yml. If you want to set other configurations, you can modify the config.js file.
- mysql
  - Data is stored using docker volume, so unless intended deletion, the data is persistent. 
  - The default application uses the root user. For security you can create a user with restricted permissions user for code-push-server to use `select,update,insert`.
- redis
  - `tryLoginTimes` Login error limit
  - `updateCheckCache` Improve application performance   

## Install docker

Official docker installation

- [>>mac](https://docs.docker.com/docker-for-mac/install/)
- [>>windows](https://docs.docker.com/docker-for-windows/install/)
- [>>linux](https://docs.docker.com/install/linux/docker-ce/ubuntu/)


`$ docker info` If installation is successfully, you can see the successful output.

## Start swarm

```shell
$ sudo docker swarm init
```

## jwt.tokenSecret modification

> code-push-server verify the json web token encryption method used by the login authentication. The symmetric encryption algorithm is public, so it is important to modify the tokenSecret value in config.js.

*Very important*

> Open `https://www.grc.com/passwords.htm` to optain `63 random alpha-numeric characters` as a key

## Deploy

```shell
$ sudo docker stack deploy -c docker-compose.yml code-push-server
```


## View progress

```shell
$ sudo docker service ls
$ sudo docker service ps code-push-server_db
$ sudo docker service ps code-push-server_redis
$ sudo docker service ps code-push-server_server
```

> Confirm `CURRENT STATE` for `Running about ...`, to make sure it has been deployed

## Simple verification of the access interface

`$ curl -I http://127.0.0.1:3000/`

return `200 OK`

```http
HTTP/1.1 200 OK
X-DNS-Prefetch-Control: off
X-Frame-Options: SAMEORIGIN
Strict-Transport-Security: max-age=15552000; includeSubDomains
X-Download-Options: noopen
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Content-Type: text/html; charset=utf-8
Content-Length: 592
ETag: W/"250-IiCMcM1ZUFSswSYCU0KeFYFEMO8"
Date: Sat, 25 Aug 2018 15:45:46 GMT
Connection: keep-alive
```

## Browser login

> Default username:admin password:123456 Remember to change the default password
> If you enter the wrong password for more than a certain number of times, you will be restricted from logging in again. To fix this you need to clear the redis cache.

```shell
$ redis-cli -p6388  # enter redis
> flushall
> quit
```


## View service log

```shell
$ sudo docker service logs code-push-server_server
$ sudo docker service logs code-push-server_db
$ sudo docker service logs code-push-server_redis
```

## View storage `docker volume ls`

| DRIVER | VOLUME NAME                   | DESCRIPTION                                |
| ------ | ----------------------------- | ------------------------------------------ |
| local  | code-push-server_data-mysql   | Database storage                           |
| local  | code-push-server_data-storage | Storage package                            |
| local  | code-push-server_data-tmp     | Temporary directory for calculating update |
| local  | code-push-server_data-redis   | redis data                                 |

## Destroy application

```bash
$ sudo docker stack rm code-push-server
$ sudo docker swarm leave --force
```
