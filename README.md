Drupal Docker Image
=============================

Simple docker image to build containers for your Drupal projects. If you are working on Drupal code you might be more interested in zaporylie/drupal-dev image which is just an extension for this image anyway.

## What you get?

* NGINX (thanks to [wiki.nginx.org](http://wiki.nginx.org/Drupal)
* php 5.6
* php-fpm (thanks to [ricardoamaro/docker-drupal-nginx](https://github.com/ricardoamaro/docker-drupal-nginx))
* all php libraries required by Drupal
* sshd
* drush
* mysql (but I strongly recommend to link separate mysql container instead)

## Some features

* autodiscovery mode (by default) will check if we have existing Drupal site in database OR if settings file exists but there is no database OR if it is brand new container with nothing in it
* numbers of environmental variables which can be used to customize container (ex. select Drupal version)
* mysql on demand (turned off by default but if you won't link separate mysql container it will be started automatically for you)
* sshd password will be autogenerated on container start. If you'd like you can set custom password instead - just use SSH_PASSWORD enviromental variable.


## Quickstart

**With** separate mysql container (recommended):

````
docker run \
  --name <drupal> \
  --link <mysql_container_name_or_id>:mysql \
  -d \
  -P \
  zaporylie/drupal
````

**Without** separate mysql container:

````
docker run \
  --name <drupal> \
  -d \
  -P \
  zaporylie/drupal
````

### How to run container in test mode?

````
docker run \
  --name <drupal> \
  --link <mysql_container_name_or_id>:mysql \
  -ti \
  -P \
  -e BUILD_TEST=1 \
  zaporylie/drupal
````

## Configuration

| ENNVIRONMENTAL VARIABLE  |  DEFAULT VALUE  |  COMMENTS  |
|:-:|:-:|:-:|
| DRUPAL_DB | drupal |  |
| DRUPAL_DB_USER | drupal |  |
| DRUPAL_DB_PASSWORD | drupal |  |
| DRUPAL_PROFILE | minimal |  |
| DRUPAL_SUBDIR | default |  |
| DRUPAL_MAJOR_VERSION | 7 |  |
| DRUPAL_DOWNLOAD_METHOD | drush |  |
| DRUPAL_GIT_BRANCH | 7.x | Only if DRUPAL_DOWNLOAD_METHOD is git |
| DRUPAL_GIT_DEPTH | 1 | Only if DRUPAL_DOWNLOAD_METHOD is git |
| METHOD | auto | Synchronization method (use drush sql-sync or file) |
| MYSQL_HOST_NAME | mysql | It is not fully supported yet |
| DRUPAL_TEST | 0 |  |
| BUILD_TEST | 0 |  |

## Dependencies (no longer required):

### Mysql

If you don't want to lose your data build data-only container first:

````
docker run \
  --name mysql_data \
  --entrypoint /bin/echo \
  mysql:5.5 \
  MYSQL data-only container
````

... then container with running mysql process ...

````
docker run \
  --name mysql_service\
  -e MYSQL_ROOT_PASSWORD=<mysecretpassword> \
  --volumes-from mysql_data \
  -d mysql:5.5
````

## Credits

This project was created by Jakub Piasecki <jakub@piaseccy.pl>

[![nodesource/node](http://dockeri.co/image/zaporylie/drupal)](https://registry.hub.docker.com/u/zaporylie/drupal/)
