---
title: Zabbix Datasheet
keywords: zabbix
last_updated: Dec 19, 2018
tags: [getting_started, zabbix]
summary: "This standard datasheet is a table of all the pertinent details related to a default Zabbix install I used during the creation of this documentation.It should provide a good start on what is needed, even if you vary from the standard (in Production, I varied much of this!)"
sidebar: zabbix_sidebar
permalink: zabbixdatasheet.html
folder: zabbix
---

## Zabbix Application Server ##

Application Name and version | Default Communication Ports | Application Dependencies 
-----------------------------|-----------------------------|------------------------
Linux CENTOS 7.x base server| NTP:UDP/123 SSH:TCP/22 SNMP:TCP/161 SMTP:TCP/587 | None
Zabbix Application Server 4.0 | Active Agent:TCP/10050, Passive Agent:TCP/10051 | PHP 7.2, Database Client Components (based on Zabbix Database Server 4.0 installation)

## Zabbix Database Server ##

Application Name and version | Default Communication Ports | Application Dependencies 
-----------------------------|-----------------------------|------------------------
Linux CENTOS 7.x base server| NTP:UDP/123 SSH:TCP/22 SNMP:TCP/161 SMTP:TCP/587 | None
Zabbix Database Server 4.0 (MySQL 8.0)| Active Agent:TCP/10050, Passive Agent:TCP/10051, Database:TCP/3306 | MySQL 8.0 Server

## Zabbix Front-End Server ##

Application Name and version | Default Communication Ports | Application Dependencies 
-----------------------------|-----------------------------|------------------------
Linux CENTOS 7.x base server| NTP:UDP/123 SSH:TCP/22 SNMP:TCP/161 SMTP:TCP/587 | None
Zabbix Web Server 4.0 | Active Agent:TCP/10050, Passive Agent:TCP/10051 | PHP 7.2, Apache Web Server, SSL

## Zabbix Proxy Server ##

Application Name and version | Default Communication Ports | Application Dependencies 
-----------------------------|-----------------------------|------------------------
Linux CENTOS 7.x base server| NTP:UDP/123 SSH:TCP/22 SNMP:TCP/161 SMTP:TCP/587 | None
Zabbix Proxy Server 4.0 | Active Agent:TCP/10050, Passive Agent:TCP/10051 | PHP 7.2, MySQL 8.0 Server


#### Zabbix Agent ####


Application Name and version | Default Communication Ports | Application Dependencies 
-----------------------------|-----------------------------|------------------------
Zabbix Agent | Active Agent:TCP/10050, Passive Agent:TCP/10051 | None