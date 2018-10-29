# INSTALLING Zabbix Agent #

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
Server=<Precompiled list of Zabbix Server/Proxies for passive checks>
ServerActive=<Precompiled list fo Zabbix Server/Proxies for active checks>
Hostname=hostname
HostMetadata=Windows
Include=c:\zabbix\templateConf\*.conf
```

- Configure Security for Zabbix Agent

**TBD**

## Linux Install ##

On Zabbix Servers, many of these steps may be bypassed, since they are already done for the installation of Zabbix itself.

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
#       List of comma delimited IP addresses, optionally in CIDR notation, or DNS names of Zabbix servers and Zabbix proxies.
#       Incoming connections will be accepted only from the hosts listed here.
Server=<ServerIP Addresses>
#       List of comma delimited Zabbix servers and Zabbix proxies for active checks.
ServerActive=<ServerIP Addresses>
Hostname=hostname
HostMetadata=Linux
Include=/etc/zabbix/zabbix_agentd.d/*.conf
```

- Configure Security for Zabbix Agent

**TBD**

RETURN TO [Zabbix Installation](.\2018-10-29-Installing_Zabbix.md) BLOG POST