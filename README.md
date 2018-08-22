# Docker-based DSFederal Intranet site

## Introduction

This repository conssists of Dockerized Drupal  website. Running Drupal 8.5, Apache 2.4, MySql 5.7 and PHP 7.These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. 

## Stack

The  stack consist of the following containers:

| Container     | Versions                | Service name    | Image                              | Default |
| ------------- | ----------------------- | --------------- | ---------------------------------- | ------- |
| [Apache+php]  | 2.4 * 7.2               | `drupalsite`   | [php:7.2-apache]                   |         |
| [mysql]       | 5.7                     | `mysqldb`       | [mysql/5.7]                        |         |

### Prerequisites


```
Git  
Docker 
```
## Reference
[GitInstallation](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
[DockerInstallation](https://docs.docker.com/install/#reporting-security-issues)



## Installation and Configuration

Download/clone repository:

```
$ git clone https://github.com/DSFederalInc/Drupal4GovCon.git
```
Change Directory 
```
$ cd Drupal4GovCon/
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

### Drupal Project Installation (Option I new installation)

The next step is to get drupal project to be part of your repository. For fresh drupal installation please run the script provided. This repo is being shipped with Drupal 8.5.5 under docroot directory. 

```
bash InstallDrupal.sh //this will remove the existing Drupal installation and pull a new 8.5.5 release 
#Please refer https://www.drupal.org/node/3060/release ton install different version of drupal and change the 
#URL on InstallDrupal.sh file to point to the version you want to install.
```


### Now go to the URL below to install Drupal on your container


```
Open: http://localhost:HOST_APACHE_PORT (The port number you configured on .env file)
```

During installation on database configuration advanced setting to  the database host to mysql container name.


### Drupal Project Installation (Option II dockerize existing installation)

To dockerize an existing project please download the project to this directory and rename it to docroot. 
It is recommended to restart the web container after any change to a volume directory. 

❗During installation if an error "Can't unlink already-existing object" occured. Please try removing the docroot directory as a root user (sudo rm -rf docroot/) and rerun this script.  


### Import existing mysql Database dump

- [x] Get Database dump
- [x] Copy the database backup to Drupal4GovCon/db-backups/

    ```
     
     $ docker exec -it mysqldb bash
     # import database from dump file. Use database name from .env file
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

### Configure Drupal config directory 

## Change config Sync direcory on Settings.php file ot 
```
$config_directories = array(
       CONFIG_SYNC_DIRECTORY => '/home/config/sync',
);
```

## To run drush command: 
```
docker exec -it drupalsite drush
```

## To run composer command: 
```
docker exec -it drupalsite composer
```


## Access your local site
Open: http://localhost:HOST_APACHE_PORT (The port number you configured on .env file)

## Resource 
[Link to DrupalGovCon Session](https://www.drupalgovcon.org/2018/program/sessions/deployment-made-easy-automating-deployment-docker)

[Link to Presentation](https://www.drupalgovcon.org/sites/default/files/session/slides/2018-08/Deployment%20Made%20Easy.pdf)

[Link to Demo I](https://www.youtube.com/watch?v=qZalwRMcQk0&feature=youtu.be)

[Link to Demo II](https://www.youtube.com/watch?v=GHTRZyfXc2M&feature=youtu.be)

## Authors

* **Hunde Keba** - *Drupal4GovCon* - [hunde](https://github.com/hunde)

* **Ashish Pagar** - *Drupal4GovCon* - [ashishpagar](https://github.com/ashishpagar)
