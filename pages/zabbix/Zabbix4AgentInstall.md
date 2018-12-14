---
title: Installing Zabbix Agent
keywords: zabbix agent
last_updated: Oct 29, 2018
tags: [getting_started, zabbix, zabbix agent]
sidebar: zabbix_sidebar
permalink: zabbixagent.html
folder: zabbix
---

The Zabbix Agent is a component that will see a LOT of variations based on your personal configuration. The below examples are set to correspond to what I needed in my environment and should be accepted as examples, and not mandatory.  In particular, setting of Hostname and/or HostMetadata will be very dependent on how you configure your
application server via the frontend.  In my case, I set it up for autoregistration assuming the machines are mostly windows (I just set a default Linux bucket rather than differentiating, though that might be added later), and that the host name is properly configured on the machine (both windows and linux).  If either of these settings to not match expectations, the server will not autoregister as expected and will hence look strange in the zabbix application.  Don't worry, there are ways to fix that too, both by adjusting it in the agent, and/or options in the application to identify it properly.

>NOTE, THIS STUFF ISN'T WORKING YET*********

Before installing the agent, if you will be securing it via certificates, be sure to secure the certificates you will use.

If this is completely internal and self-signed certificates are sufficient for your purposes, use the following procedure:

- Create the Self-Signed Cert (INTERNAL USE ONLY)

```bash
$mkdir -p /etc/zabbix/private_agent
$chmod 700 /etc/zabbix/private_agent
$openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/zabbix/private_agent/agent-selfsigned.key -out /etc/zabbix/agent-selfsigned.crt
```

>NOTE, ABOVE ISN'T WORKING YET*********

## Windows Install ##

Since Zabbix is a Linux system, it isn't as easy to install the service for windows as it is for Linux.  However, it is easy to download the precompiled agent and use that with the configuration file.

- Download the Zabbix Precompiled Windows Agent

- Configure the conf file for core settings

```text
# Edit the following lines to match the required settings listed below:
LogFile=C:\Zabbix\logs\zabbix_agentd.log
#At a minimum, edit the following lines to point to the appropriate proxy and/or server
PidFile=/var/run/zabbix/zabbix_agentd.pid
LogFile=/var/log/zabbix/zabbix_agentd.log
LogFileSize=0
EnableRemoteCommands=1
LogRemoteCommands=1
#       List of comma delimited IP addresses, optionally in CIDR notation of Zabbix servers and Zabbix proxies.
#       Incoming connections will be accepted only from the hosts listed here.
Server=<ServerIP Addresses> # NOTE:  DNS NAMES DO NOT SEEM TO WORK, AT LEAST IN OUR ENVIRONMENT.  MAY BE DNS RELATED, OR A ZABBIX ISSUE - DID NOT TRY TO DETERMINE
#       List of comma delimited Zabbix servers and Zabbix proxies for active checks.
ServerActive=<ServerIP Addresses> # NOTE:  RECOMMEND ONLY ONE SERVER IDENTIFIED TO PREVENT CONFUSION
Hostname=Zabbix Server #  <--REMOVE OR COMMENT OUT THIS LINE - IT WILL PULL FROM HOSTNAME
HostMetadata=<Appropriate Metadata Tag as defined in Frontend>
Include=/etc/zabbix/zabbix_agentd.d/*.conf
```

- Configure Security for Zabbix Agent

**TBD**

## Linux Install ##

On Zabbix Servers, many of these steps may be bypassed, since they are already done for the installation of Zabbix itself.

### RedHat\CENTOS 7.x ###

Other RedHat versions are similar.  See the Zabbix documentation to determine the correct repo to install.

- Install the Zabbix Repository

```bash
$rpm -ivh https://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-1.el7.noarch.rpm
```

- Install Zabbix Agent

```bash
$yum -y install zabbix-agent
```

- Configure the Zabbix Agent

```bash
$vim /etc/zabbix/zabbix_agentd.conf

#At a minimum, edit the following lines to point to the appropriate proxy and/or server
PidFile=/var/run/zabbix/zabbix_agentd.pid
LogFile=/var/log/zabbix/zabbix_agentd.log
LogFileSize=0
EnableRemoteCommands=1
LogRemoteCommands=1
#       List of comma delimited IP addresses, optionally in CIDR notation of Zabbix servers and Zabbix proxies.
#       Incoming connections will be accepted only from the hosts listed here.
Server=<ServerIP Addresses> # NOTE:  DNS NAMES DO NOT SEEM TO WORK, AT LEAST IN OUR ENVIRONMENT.  MAY BE DNS RELATED, OR A ZABBIX ISSUE - DID NOT TRY TO DETERMINE
#       List of comma delimited Zabbix servers and Zabbix proxies for active checks.
ServerActive=<ServerIP Addresses> # NOTE:  RECOMMEND ONLY ONE SERVER IDENTIFIED TO PREVENT CONFUSION
Hostname=Zabbix Server #  <--REMOVE OR COMMENT OUT THIS LINE - IT WILL PULL FROM HOSTNAME
HostMetadata=Linux
Include=/etc/zabbix/zabbix_agentd.d/*.conf
```

- Configure Security for Zabbix Agent

**TBD**

### Ubuntu 16.04.x ###

Other Debian-based versions are similar.  See the Zabbix documentation to determine the correct repo and installation commands to install.

- Install the Zabbix Repository

```bash
$wget https://repo.zabbix.com/zabbix/4.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.0-2+xenial_all.deb
$dpkg -i zabbix-release_4.0-2+xenial_all.deb
$apt update
```

- Install Zabbix Agent

```bash
$apt install zabbix-agent
```

- Configure the Zabbix Agent

```bash
$vim /etc/zabbix/zabbix_agentd.conf

#At a minimum, edit the following lines to point to the appropriate proxy and/or server
PidFile=/var/run/zabbix/zabbix_agentd.pid
LogFile=/var/log/zabbix/zabbix_agentd.log
LogFileSize=0
EnableRemoteCommands=1
LogRemoteCommands=1
#       List of comma delimited IP addresses, optionally in CIDR notation of Zabbix servers and Zabbix proxies.
#       Incoming connections will be accepted only from the hosts listed here.
Server=<ServerIP Addresses> # NOTE:  DNS NAMES DO NOT SEEM TO WORK, AT LEAST IN OUR ENVIRONMENT.  MAY BE DNS RELATED, OR A ZABBIX ISSUE - DID NOT TRY TO DETERMINE
#       List of comma delimited Zabbix servers and Zabbix proxies for active checks.
ServerActive=<ServerIP Addresses> # NOTE:  RECOMMEND ONLY ONE SERVER IDENTIFIED TO PREVENT CONFUSION
Hostname=Zabbix Server #  <--REMOVE OR COMMENT OUT THIS LINE - IT WILL PULL FROM HOSTNAME
HostMetadata=Linux
Include=/etc/zabbix/zabbix_agentd.d/*.conf
```

- Configure Security for Zabbix Agent

**TBD**

---