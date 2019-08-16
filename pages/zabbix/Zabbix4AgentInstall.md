---
title: Installing Zabbix Agent
keywords: zabbix agent
last_updated: Aug 12, 2019
tags: [getting_started, zabbix, zabbix agent]
sidebar: zabbix_sidebar
permalink: zabbixagent.html
folder: zabbix
---

The Zabbix Agent is a component that will see a LOT of variations based on your personal configuration. The below examples are set to correspond to what I needed in my environment and should be accepted as examples, and not mandatory. In particular, setting of Hostname and/or HostMetadata will be very dependent on how you configure your
application server via the frontend. In my case, I set it up for auto-registration assuming the machines are mostly windows (I just set a default Linux bucket rather than differentiating, though that might be added later), and that the host name is properly configured on the machine (both windows and linux). If either of these settings to not match expectations, the server will not auto-register as expected and will hence look strange in the zabbix application. Don't worry, there are ways to fix that too, both by adjusting it in the agent, and/or options in the application to identify it properly.

## Windows Install ##

Since Zabbix is a Linux system, it isn't as easy to install the service for windows as it is for Linux. However, it is easy to download the precompiled agent and use that with the configuration file.

- Download the Zabbix Precompiled Windows Agent

- Configure the conf file for core settings

```bash
# Edit c:\zabbix\zabbix_agentd.conf file in an Elevated Notepad session (or equivalent):

# Edit the following lines to match the required settings listed below:
PidFile=/var/run/zabbix/zabbix_agentd.pid
LogFile=/var/log/zabbix/zabbix_agentd.log
LogFileSize=0
EnableRemoteCommands=1
LogRemoteCommands=1
Server=<ServerIP Address(es)>
ServerActive=<ServerIP Address of Proxy to use.> # NOTE:  If no proxy is used, set to zabbix server
Hostname=Zabbix Server #  <--REMOVE OR COMMENT OUT THIS LINE - IT WILL PULL FROM THE HOSTNAME
HostMetadata=<Appropriate Metadata Tag as defined in Auto-Registration Actions - See Metadata Configuration Settings below>
Include=/etc/zabbix/zabbix_agentd.d/*.conf
```

- Open Firewall Ports 10050 and 10051 in the Windows Firewall (if enabled)

```text
netsh.exe advfirewall firewall add rule name=”Zabbix Monitoring" dir=in action=allow protocol=TCP localport=10050-10051
netsh.exe advfirewall firewall add rule name=”Zabbix Monitoring" dir=out action=allow protocol=TCP localport=10050-10051
```

- Start/Verify Zabbix Agent Service is running

```bash
# NOTE that the path may vary based on the version of Zabbix Agent used, and where you place it

c:\Zabbix\bin\win\zabbix_agentd.exe -c c:\Zabbix\zabbix_agentd.conf -i
c:\Zabbix\bin\win\zabbix_agentd.exe -s
```

## Linux Install ##

On Zabbix Servers, many of these steps may be bypassed, since they are already done for the installation of Zabbix itself.

### RedHat\CENTOS 7.x ###

Other RedHat versions are similar. See the Zabbix documentation to determine the correct repo to install.

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
Server=<ServerIP Address(es)>
ServerActive=<ServerIP Address of Proxy to use.> # NOTE:  If no proxy is used, set to zabbix server
Hostname=Zabbix Server #  <--REMOVE OR COMMENT OUT THIS LINE - IT WILL PULL FROM THE HOSTNAME
HostMetadata=<Appropriate Metadata Tag as defined in Auto-Registration Actions - See Metadata Configuration Settings below>
Include=/etc/zabbix/zabbix_agentd.d/*.conf
```

- Open Firewall Ports

```bash
$firewall-cmd --permanent --add-port=10050/tcp
$firewall-cmd --permanent --add-port=10051/tcp
$firewall-cmd --reload
```

- Start Zabbix Agent

```bash
$systemctl start zabbix-agent
$systemctl enable zabbix-agent
```

### Ubuntu 16.04.x ###

Other Debian-based versions are similar.See the Zabbix documentation to determine the correct repo and installation commands to install.

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
Server=<ServerIP Address(es)>
ServerActive=<ServerIP Address of Proxy to use.> # NOTE:  If no proxy is used, set to zabbix server
Hostname=Zabbix Server #  <--REMOVE OR COMMENT OUT THIS LINE - IT WILL PULL FROM THE HOSTNAME
HostMetadata=<Appropriate Metadata Tag as defined in Auto-Registration Actions - See Metadata Configuration Settings below>
Include=/etc/zabbix/zabbix_agentd.d/*.conf
```

- Start Zabbix Agent

```bash
$systemctl start zabbix-agent
$systemctl enable zabbix-agent
```

## Metadata Configuration Settings ##

Metadata Configuration is a string of tags used in Auto-registration to apply the correct templates to the host and add
the host to all appropriate host groups. What you make mandatory vs optional is up to you. At a minimum, I recommend any
field used to apply templates to all servers should be mandatory fields. Application specific templates, for example,
would not apply to this. In fact, if it is application specific, I would not make that field mandatory at all, since not
all servers would be pegged to a specific application. Some examples include test servers, utility servers, or shared
services servers - ones that server several purposes. Either way, this flexible field can be customized to whatever fits
your needs. The way I use this field is to break down the following information (The first 3 are mandatory):

What Operating System is used? (Used for applying to appropriate OS template and adding to an OS Host Group)
    Linux
    Window

What type of machine is this? (Used for applying VM Template and Virtual Host Group, if appropriate)
    Physical
    Virtual

What purpose does this server have - what is it's function? (Used to apply application specific templates and Host Group)
    DomainController
    WebServer
    AppServer
    MySQL

What Application or Team does this server support? (Used to apply Team specific Host Groups and templates, if any)
    Zabbix
    SharedServices
    Payroll

Where is the System physically located? (Used for applying Location Host Group, and any location specific templates)
    US
    Mexico
    Europe
    Texas

To see how it is applied, an example would be most helpful.  Here is an example of the Metadata field line in a Payroll
Server located in Texas:

```bash
Metadata=Windows Virtual AppServer Payroll Texas
```

It will work equally with or without dilimiters, as long as the case and letters match.  So this would be effectively 
equivalent:

```bash
Metadata=WindowsVirtualAppServerPayrollTexas
```

or even,

```bash
Metadata=Windows,Virtual,AppServer,Payroll,Texas
```

All seem to work fine, but for readability, I stick with the space delimited version as a personal preference.

---