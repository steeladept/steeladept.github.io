---
title: Installing Zabbix Web Interface on CENTOS 7 using Apache Web Server
keywords: zabbix server centos apache
last_updated: Oct 29, 2018
tags: [getting_started, zabbix, zabbix web, apache]
sidebar: zabbix_sidebar
permalink: zabbixweb.html
folder: zabbix
---

## Installing CENTOS 7 ##

>The assumption is you already have CENTOS 7 installed, connected to the internet and network location(s), and updated to current patch level.

This document will take the user from an initial CENTOS 7 setup to a full Zabbix installation.  All steps should be included and anything missing should be added or relayed to the author for addition.

## Before Beginning ##

Make sure the Database Zabbix will be using is already installed.  See my [MySQL Install Guide](./Zabbix4MySQLInstall.md) for instructions on installing MySQL for Zabbix.
Further, make sure the application server is already installed.  See my [Zabbix Application Server Installation Guide](./Zabbix4AppServerInstall.md) for instructions on setting up the Application Server.

NOTE:  These may be on the same or separate servers, but you need to ensure they exist first in either case.

## Installing Zabbix Prerequisites ##

- Install Apache Web Server Components

```bash
$yum -y install httpd
```

- Install the Zabbix Repository

```bash
$rpm -ivh https://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-1.el7.noarch.rpm
```

- Install Zabbix Packages

```bash
$yum-config-manager --enable rhel-7-server-optional-rpms
$yum -y install zabbix-web-mysql zabbix-agent
```

>NOTE: If the Remi-PHP packages are already installed from an earlier install, you may run into a dependency issue with the remi-php72 packages.  To resolve, type **yum remove php**, and then install the web server.  After it is complete, verify PHP 7.2+ is still being used by typing **php -v**

- Install PHP repository

```bash
$yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

- Install PHP packages

```bash
$yum-config-manager --enable remi-php72
$yum -y update
```

## Configuring PHP for Zabbix ##

- Edit the PHP.ini file

```bash
$vim /etc/php.ini
```

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

## Configuring Frontend ##

- Install mod_ssl to be able to secure the frontend server

```bash
$yum -y install mod_ssl
```

- Create the Self-Signed Cert (INTERNAL USE ONLY)

```bash
$mkdir -p /etc/httpd/ssl/private
$chmod 700 /etc/httpd/ssl/private
$openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/httpd/ssl/private/apache-selfsigned.key -out /etc/httpd/ssl/apache-selfsigned.crt
```

- Edit the Apache SSL Configuration

```bash
$vim /etc/httpd/conf.d/ssl.conf
```

  Make it match the ssl locations and configurations just created:

> Note:  If Apache is only being used for Zabbix, you can just edit the default VirtualHost.  Otherwise, create one specifically for Zabbix.

```vim
DocumentRoot "/usr/share/zabbix"
ServerName example.com:443
SSLProtocol -all +TLSv1.2
SSLCertificateFile /etc/httpd/ssl/apache-selfsigned.crt
SSLCertificateKeyFile /etc/httpd/ssl/private/apache-selfsigned.key
```

- Enable Zabbix on root directory of URL

```bash
$vim /etc/httpd/conf.d/zabbix.conf

#Edit entries to match PHP entries above
php_value max_execution_time 600
php_value memory_limit 256M
php_value post_max_size 32M
php_value upload_max_filesize 16M
php_value max_input_time 600
#Uncomment the following line and change to UTC as shown
php_value date.timezone Etc/UTC
```

- Additional Hardening for Production Environments. See [Zabbix Documentation](https://www.zabbix.com/documentation/4.0/manual/installation/requirements/best_practices "Best Practices") (I am using in test too, but you can comment out if troubleshooting)

```bash
$vim /etc/httpd/conf/httpd.conf

#Add the following entries
<VirtualHost *:*>
    ServerName <SERVERNAME>
    Redirect permanent / http://<SERVERNAME>.com
</VirtualHost>

ServerSignature Off
ServerTokens Prod
```

- Update SELinux configurations

```bash
$cd /etc/zabbix
$setsebool -P httpd_can_connect_zabbix on
$setsebool -P httpd_can_network_connect_db on

#if database connection errors are seen, try changing to permissive
$setenforce permissive

#if that works, troubleshoot source or change permanently
$vim /etc/sysconfig/selinux
```

- Update Firewalld settings

```bash
$firewall-cmd --permanent --add-service https
$firewall-cmd --reload
$firewall-cmd --list-all
```

- Finally, enable the Apache Service to start on reboot

```bash
$systemctl enable httpd
$systemctl restart httpd
```

## Configure Zabbix ##

- Go to the website and verify it is working

```http
https://<IP_Address>/zabbix
```

- Verify the DNS, if appropriate

```http
https://<DNS_Web_Address>/zabbix
```

- Log in with initial Admin account
  - User ID = Admin
  - Password = zabbix

- Change the Admin Password/Account

---

## Next Steps ##

- Install the agent.  See [Agent Configuration Guide](./Zabbix4AgentInstall.md) for details.

- Configure Proxy Servers, if used.
  - Start with the [MySQL Install Guide](./Zabbix4MySQLInstall.md) for instructions on installing MySQL for Zabbix Proxy Servers.
  - Follow with the [Zabbix Proxy Installation Guide](./Zabbix4ProxyInstall.md) for instructions on setting up Proxy Servers.

- Configure Zabbix and/or add machines for monitoring as described in the [Zabbix Administration Guide](./ZabbixAdministration.md).

---