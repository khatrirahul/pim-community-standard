Akeneo PIM Community Standard Edition
=====================================

Welcome to Akeneo PIM Product.

This repository is used to create a new PIM project based on Akeneo PIM.

If you want to contribute to the Akeneo PIM (and we will be pleased if you do!), you can fork the repository https://github.com/akeneo/pim-community-dev and submit a pull request.

Scrutinizer | Crowdin
----------- | -------
[![Scrutinizer Quality Score](https://scrutinizer-ci.com/g/akeneo/pim-community-dev/badges/quality-score.png?s=05ef3d5d2bbfae2f9a659060b21711d275f0c1ff)](https://scrutinizer-ci.com/g/akeneo/pim-community-dev/) | [![Crowdin](https://d322cqt584bo4o.cloudfront.net/akeneo/localized.svg)](https://crowdin.com/project/akeneo)

Application Technical Information
---------------------------------

The following documentation is designed for both clients and partners and provides all technical information required to define required server(s) to run Akeneo PIM application and check that end users workstation is compatible with Akeneo PIM application:
https://docs.akeneo.com/master/install_pim/manual/system_requirements/system_requirements.html

Installation instructions
-------------------------
(skip to `QUICK SETUP` below for step by step instructions)

### Recommended installation

To install Akeneo PIM for a PIM project or for evaluation, please follow: https://docs.akeneo.com/master/install_pim/manual/installation_ce_archive.html

### Using Composer to create the project

Alternatively, you can install Akeneo PIM with Composer, but please make sure that all requirements are fulfilled.

If you don't have Composer yet, download it following the instructions on http://getcomposer.org/ or just run the following command:

```
    $ curl -s https://getcomposer.org/installer | php
```


After that, follow the instructions here:

Manual: https://docs.akeneo.com/master/install_pim/manual/index.html
Docker: https://docs.akeneo.com/master/install_pim/docker/installation_docker.html

Upgrade instructions
--------------------

To upgrade Akeneo PIM to a newer version, please follow:
https://docs.akeneo.com/master/migrate_pim/index.html


QUICK SETUP
-----------
**Note**:  These steps will setup Akeneo PIM in `prod` environment.

Pre-requisites: 
--------------
**On Linux hosts it is mandatory that the user (using which you are going to run below given commands) of your host machine has 1000:1000 as UID and GID too, otherwise you’ll end up with a non-working PIM due to permission issues.**

There are two types of installations one is icecat_demo_dev (with demo files) and other is minimal (without demo files)

Below are the common steps for both installations
-----------------------------
Run below given commands:
```
mkdir akeneo
cd akeneo
git clone https://github.com/khatrirahul/pim-community-standard.git
curl -s https://getcomposer.org/installer | php
cd pim-community-standard/
cp .env.dist .env
vim app/config/parameters.yml.dist
```

Change below given two parameters only and save the file:

`database_host:                        localhost`
to
`database_host:                        mysql`

`index_hosts:                          'localhost: 9200'`
to
`index_hosts:                          'elastic:changeme@elasticsearch:9200'`

```
cp  app/config/parameters.yml.dist  app/config/parameters.yml
docker-compose up -d
sh ./bin/docker/pim-dependencies.sh
```
For full installation (with demo files)
------
```
sh ./bin/docker/pim-initialize.sh
```
For minimal installation (without demo files)
------
```
vim app/config/pim_parameters.yml  
```
Change the following parameters:
-----

		    installer_data:       PimInstallerBundle:icecat_demo_dev
			 
		to:
		    installer_data:       PimInstallerBundle:minimal

```
sh ./bin/docker/pim-initialize.sh
```
exec inside the fpm:php container with
----
```
docker exec -it 'container_id' bash
```
run below command inside container
------
```
php bin/console pim:user:create --admin -e prod -n -- admin admin test@example.com John Doe en_US`
```

Open browser and access `localhost:8080`
----
```
user: admin
password: admin
```
