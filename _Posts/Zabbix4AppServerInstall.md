# INSTALLING Zabbix Application Server ON CENTOS 7 #

## Installing CENTOS 7 ##

>The assumption is you already have CENTOS 7 installed, connected to the internet and network location(s), and updated to current patch level.

This document will take the user from an initial CENTOS 7 setup to a full Zabbix installation.  All steps should be included and anything missing should be added or relayed to the author for addition.

## Before Beginning ##

Make sure the database Zabbix will be using is already installed.  See my [MySQL Install Guide](./Zabbix4MySQLInstall.md) for instructions on installing MySQL for Zabbix.

NOTE:  This may be on the same or separate servers, but you need to ensure it exist first in either case.

## Installing Zabbix Prerequisites ##

- Install the Zabbix Repository

```bash
$rpm -ivh https://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-1.el7.noarch.rpm
```

- Download the mysql repository

```bash
$wget https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm
$rpm -ivh mysql80-community-release-el7-1.noarch.rpm
```

- Install the MySQL Client application *used for testing/troubleshooting connections to remote database*

```bash
$yum -y install mysql-community-client
```

- Install Zabbix Packages

```bash
$yum-config-manager --enable rhel-7-server-optional-rpms
$yum -y install zabbix-server-mysql zabbix-agent zabbix-get
```

- Install PHP repository

```bash
$yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

- Install PHP packages

```bash
$yum-config-manager --enable remi-php72
$yum -y install php
```

## Configuring PHP for Zabbix ##

- Edit the PHP.ini file

```bash
$vim /etc/php.ini
```

>*May also need to change in: /etc/httpd/conf.d/zabbix.conf*

Change the values to match the configuration shown:

```vim
max_execution_time = 600
max_input_time = 600
memory_limit = 256M
post_max_size = 32M
upload_max_filesize = 16M
date.timezone = UTC
expose_php = Off
```

- Edit the Zabbix Configuration file

```bash
$vim /etc/zabbix/zabbix_server.conf
```

- Edit the following lines with the appropriate entries:

```vim
DBHost=<hostIPAddr>
DBName=zabbix
DBUser=zabbix
DBPassword=<database password>
```

- Start the Zabbix server

```bash
$systemctl start zabbix-server
$systemctl enable zabbix-server
```

- Verify the server is working

```bash
$systemctl status zabbix-server
```

### Known Issues and Resolution ###

1. It is common for SELinux to get in the way of connecting to remote services, and connecting to an external MySQL database, I kept running into this issue:

  1166:20181028:033532.402 [Z3001] connection to database 'zabbix' failed: [2003] Can't connect to MySQL server on 'mysqlserver.com' (13)
  1166:20181028:033532.402 database is down: reconnecting in 10 seconds

To fix this issue, do the following:

- Update SELinux configurations

```bash
$cd /etc/zabbix
$setsebool -P httpd_can_connect_zabbix on
$setsebool -P httpd_can_network_connect_db on

#if database connection errors are seen, try changing to permissive
$setenforce permissive

#if that works, troubleshoot source or change to permissive permanently
$vim /etc/sysconfig/selinux
```

2. Second issue yet to be defined.

Do not forget to install the agent.  See [Agent Configuration Guide](./Zabbix4AgentInstall.md) for details.  

>Note that you may need to come back and add or adjust this after you install the front end web server so you can verify functionality

## Next Steps ##

Now the Database is prepared.  See my [Zabbix Frontend Installation Guide](./Zabbix4FrontEndInstall.md) for instructions on setting up the web server interface.

RETURN TO [Zabbix Installation](.\2018-10-29-Installing_Zabbix.md) BLOG POST