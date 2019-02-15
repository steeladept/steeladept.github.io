---
title: Zabbix Datasheet
keywords: zabbix
last_updated: Feb 14, 2019
tags: [getting_started, zabbix]
summary: "This standard datasheet is a table of all the pertinent details related to a default Zabbix install I used during the creation of this documentation.It should provide a good start on what is needed, even if you vary from the standard (in Production, I varied much of this!)."
sidebar: zabbix_sidebar
permalink: zabbixdatasheet.html
folder: zabbix
---

One point to remember on the server sizing listed below. While the initial sizing is listed in the main documentation, this sheet maintains the actual size used in a 1000+ machine environment requiring over 1400 vps using proxy servers at each major datacenter. Server sizing, particularly disk space, varies greatly on the size of the environment monitored. (I found CPU and Memory were not heavily taxed, because the default configuration files limited utilization of these resources. Changing the configuration files to take more advantage of the resources will increase utilization, and probably performance)

## Zabbix Application Server ##

Application Name and version | Default Communication Ports | Application Dependencies
-----------------------------|-----------------------------|------------------------
Linux CENTOS 7.x base server| NTP:UDP/123 SSH:TCP/22 SNMP:TCP/161 SMTP:TCP/587 | None
Zabbix Application Server 4.0 | Active Agent:TCP/10050, Passive Agent:TCP/10051 | PHP 7.2, Database Client Components (based on Zabbix Database Server 4.0 installation)

Server Sizing - CPU | Server Sizing - Memory | Server Sizing Disk Space
--------------------|------------------------|------------------------
1 vCPU | 8 GB | 40GB - OS Drive


## Zabbix Database Server ##

Application Name and version | Default Communication Ports | Application Dependencies 
-----------------------------|-----------------------------|------------------------
Linux CENTOS 7.x base server| NTP:UDP/123 SSH:TCP/22 SNMP:TCP/161 SMTP:TCP/587 | None
Zabbix Database Server 4.0 (MySQL 8.0)| Active Agent:TCP/10050, Passive Agent:TCP/10051, Database:TCP/3306 | MySQL 8.0 Server

Server Sizing - CPU | Server Sizing - Memory | Server Sizing Disk Space
--------------------|------------------------|------------------------
2 vCPUs | 24 GB | 40GB - OS Drive, 250GB - Data Drive

## Zabbix Front-End Server ##

Application Name and version | Default Communication Ports | Application Dependencies 
-----------------------------|-----------------------------|------------------------
Linux CENTOS 7.x base server| NTP:UDP/123 SSH:TCP/22 SNMP:TCP/161 SMTP:TCP/587 | None
Zabbix Web Server 4.0 | Active Agent:TCP/10050, Passive Agent:TCP/10051 | PHP 7.2, Apache Web Server, SSL

Server Sizing - CPU | Server Sizing - Memory | Server Sizing Disk Space
--------------------|------------------------|------------------------
1 vCPU | 4 GB | 40GB - OS Drive

## Zabbix Proxy Server ##

Application Name and version | Default Communication Ports | Application Dependencies 
-----------------------------|-----------------------------|------------------------
Linux CENTOS 7.x base server| NTP:UDP/123 SSH:TCP/22 SNMP:TCP/161 SMTP:TCP/587 | None
Zabbix Proxy Server 4.0 | Active Agent:TCP/10050, Passive Agent:TCP/10051 | PHP 7.2, MySQL 8.0 Server

Server Sizing - CPU | Server Sizing - Memory | Server Sizing Disk Space
--------------------|------------------------|------------------------
1 vCPU | 8 GB | 100GB - OS Drive

#### Zabbix Agent ####

Application Name and version | Default Communication Ports | Application Dependencies 
-----------------------------|-----------------------------|------------------------
Zabbix Agent | Active Agent:TCP/10050, Passive Agent:TCP/10051 | None