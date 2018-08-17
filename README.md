# Docker-based DSFederal Intranet site

## Introduction

This repository conssists of Dockerized DSFederal Intranet website. Running Drupal 8.5, Apache 2.4, MySql 5.7 and PHP 7
Read [**Getting Started**](http://wodby.com/stacks/drupal/docs/local/quick-start).

## Stack

The  stack consist of the following containers:

| Container     | Versions                | Service name    | Image                              | Default |
| ------------- | ----------------------- | --------------- | ---------------------------------- | ------- |
| [Apache+php]  | 2.4 * 7.2               | `dsfintranet`   | [php:7.2-apache]                   |         |
| [mysql]       | 5.7                     | `mysqldb`       | [mysql/5.7]                        |         |

## Installation and Configuration

Download/clone repository:

```
$ git clone git@bitbucket.org:dsfederal/d8intranet.git
```
Change Directory 
```
$ cd d8intranet/
```

###### Edit Key Configuration files 
  ** .env file ** 
    This is file consist  enviromental varialbles:
    
        ```
        DB_NAME=Database Name
        DB_USER=Database username
        DB_PASSWORD=Database password for $DB_USER
        DB_ROOT_PASSWORD=Mysql Root Password
        DB_HOST=HostNamae 
        DB_DRIVER=mysql
        MYSQL_VERSION=5.7 (Mysql Version/ Recommended to leave it with 5.7)
        HOST_MYSQL_PORT= Docker container Mapping port for mysql
        HOST_APACHE_PORT= Docker container Mapping port for apache
         ```
         
The repository will have three more file to store environmental specific varailble (_envDev,_envStage & _envProd). For automated deployment
it is recomneded to change file specific to the environment. For local development .evn can be edited to match local configuration. 

Build and run detached docker
 ```
 $ docker-compose up --build -d
```
###### Import  mysql database dump

- [x] Get Database dump
- [x] Copy the database backup to d8intranet/db-backups/

    ```
     
     $ docker exec -it mysqldb bash
     # create database with desired name
     $ root@container_id:/# mysql -u root -p "create database database_name;"
     # import database from dump file
     $ root@container_id:/# mysql -u root -p database_name < /var/mysql/backups/database_dump.sql
     $ root@container_id:/# exit
 
    ```
  
###### Configure settings.php file 

    ```
      #docroot/sites/default/settings.php
      $databases['default']['default'] = array (
     'database' => 'Dbname',
     'username' => 'Dbusername',
     'password' => 'Dbpassword',
     'prefix' => '',
     'host' => 'mysqldb',
     'port' => '3306',
     'namespace' => 'Drupal\\Core\\Database\\Driver\\mysql',
     'driver' => 'mysql',
     );
    ```
## Change config Sync direcory on Settings.php file ot 
```
$config_directories = array(
       CONFIG_SYNC_DIRECTORY => '/home/config/sync',
);
```

## Drush 
```
docker exec -it dsfintranet drush
```
## Access your local site
Open: http://localhost:HOST_APACHE_PORT (The port number you configured on .env file)


