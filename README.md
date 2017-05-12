 Vagrant box with OpenLDAP
======

this project will perform the following steps:
*  install openldap
*  set up the "MemberOf" overlays
*  create a forest "dc=jfrog,dc=com"
*  create 3 groups : soleng, avengers, justice_league
*  add some users in each groups

OS : centos7

## Requirements 
* Virtual Box 
* Vagrant 

## Usage

check out the project

```
$ git clone https://github.com/cyan21/vagrant_openldap.git
```

## How-to use this code

unzip the archive which contains the ldif files and helper files

```
$ cd vagrant_openldap
$ tar -xzvf ldap_config.tar.gz
```

spin up your vagrant box

```
$ vagrant up
```
