---
title: Installing Zabbix Proxy Server on CENTOS 7
keywords: zabbix proxy centos
last_updated: Jan 8, 2019
tags: [getting_started, zabbix, zabbix proxy]
sidebar: zabbix_sidebar
permalink: zabbixproxy.html
folder: zabbix
---

## Installing CENTOS 7 ##

>The assumption is you already have CENTOS 7 installed, connected to the internet and network location(s), and updated to current patch level. In addition, this document assumes MySQL is already installed and configured for the PROXY. If not, use the directions to install [MySQL for Zabbix](./Zabbix4MySQLInstall.md)

This document will take the user from an intial CENTOS 7 setup to a full Zabbix installation. All steps should be included and anything missing should be added or relayed to the author for addition.

## Before Beginning ##

>Make sure the database the proxy will be using is already installed. See my [MySQL Install Guide](./Zabbix4MySQLInstall.md) for instructions on installing MySQL for Zabbix Proxies.

NOTE:  This typically will be on the same servers, but can be separated if necessary. Either way, you need to ensure it exist first.

## Installing Zabbix Prerequisites ##

- Install the Zabbix Repository

```bash
$rpm -ivh https://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-1.el7.noarch.rpm
```

- Install Zabbix Packages

```bash
$yum-config-manager --enable rhel-7-server-optional-rpms
$yum -y install zabbix-proxy-mysql zabbix-agent zabbix-get
```

- Install PHP repository

```bash
$yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

- Install PHP packages

```bash
$yum-config-manager --enable remi-php72
$yum -y update
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
$vim /etc/zabbix/zabbix_proxy.conf
```

- Edit the following lines with the appropriate entries:

```vim
DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=<database password>
```

- Start the Zabbix Proxy

```bash
$systemctl start zabbix-proxy
$systemctl enable zabbix-proxy
```

- Verify the server is working

```bash
$systemctl status zabbix-proxy
```

## Add Proxy to Server ##

---

## Next Steps ##

- Configure Zabbix and/or add machines for monitoring as described in the [Zabbix Administration Guide](./ZabbixAdministration.md).

---