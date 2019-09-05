---
title: Installing MySQL 8.0 on CENTOS 7 for Zabbix
keywords: zabbix database centos
last_updated: Sept 4, 2019
tags: [getting_started, zabbix, zabbix database]
sidebar: zabbix_sidebar
permalink: zabbixdb.html
folder: zabbix
---

## Installing CENTOS 7 ##

>The assumption is you already have CENTOS 7 installed, connected to the internet and network location(s), and updated to current patch level.

This document will take the user from an initial CENTOS 7 setup to a full Zabbix installation.All steps should be included and anything missing should be added or relayed to the author for addition.

## Add additional resources (if necessary) ##

### CPU/MEMORY ###

- These should be added in the VMware Console before starting the machine, if possible.Hot-add is possible, but could lead to stability issues and force a reboot.

**Zabbix Server**:
> Configure to a minimum of 2 CPU and 24GB RAM

**Zabbix Proxy**:
> Configure to a minimum of 1 CPU and 8GB RAM

### Hard Drive Space (Zabbix Server Database only) ###

- Add 2nd Hard Drive to VM in VMware

- Add Hard Drive in Linux

```bash
# First, determine device name (most likely sdb)
ls -1 /dev/sd*

# Once you find it, issue the following commands changing the name of the hard drive as necessary
mkfs.xfs /dev/sdb
```

> We will mount this at the data store mount point after installing mysql and before starting it up

## Installing Zabbix Prerequisites ##

- Download the mysql repository

```bash
$wget https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm
$rpm -ivh mysql80-community-release-el7-1.noarch.rpm
```

- Install the database - *(MySQL is used here, but MariaDB or Postgres can be substituted - check Zabbix documentation for version requirements)*

```bash
$yum -y install mysql-community-server
```

- (Zabbix Application Database Server Only! *not proxy databases*) Mount the new hard drive to /var/lib/mysql and configuration in /etc/fstab to make it permanent

```bash
$mount /dev/sdb /var/lib/mysql
$ vim /etc/fstab

#add the following line to the bottom (assuming the device name (sdb) is valid)
/dev/sdb                                  /var/lib/mysql                   xfs     defaults        0 0
```

- (External Databases Only!) Open the firewall port to allow connection to mysql

```bash
$firewall-cmd --permanent --add-port=3306/tcp
$firewall-cmd --reload
$firewall-cmd --list-all
```

- Setup MySQL Database

  *NOTE: On CENTOS7, MySQL generates a temporary database root password written to the log in /var/log/mysqld.log after initial startup. As shown in the steps below, get this password before running the mysql_secure_connection script or it will throw an error*

  *NOTE: I am using mysql root user password of MySQLrootpass\*1 (in test only)*
  
```bash
$systemctl start mysqld
$grep 'temporary password' /var/log/mysqld.log
$mysql_secure_installation
#Enter password from log file
#Change password
#Select 'no' when prompted to change password (you just did it, no need to do so again)
#Select yes, to all the rest of the questions:
    #Remove anonymous users
    #Disallow remote ROOT access
    #Remove test databases
    #Reload privilege tables
```

> The base install of MySQL is now complete

## Installing Zabbix Database ##

- Install Zabbix packages as described in the [Zabbix Application Server](./Zabbix4AppServerInstall.md) installation or [Zabbix Proxy Server](./Zabbix4ProxyInstall.md) installation instructions as appropriate, if not already completed.  

NOTE: For a standalone SQL Server, install the Zabbix Repository, then zabbix-server-mysql and zabbix-agent only. No other packages are necessary for the database server.

- Edit the MySQL Configuration file to change the way the passwords work (needed only for MySQL 8+ due to a change in how the password encryption is done)

```bash
$vim /etc/my.cnf

# Change the default directory for the database data (if desired)


# add the following lines to the config file (Can't do earlier or we can't run the secure_install script):
skip_name_resolve
default_authentication_plugin=mysql_native_password
event_scheduler=ON
expire_logs_days=7
```

- Add Zabbix User to database

  *NOTE: I am using a zabbix database password of Zabdbpass\*1 (in test only)*
  
Note that '\<serverID>' is the host name of the server that will connect.  For end user accounts, set it equal to '%' to allow it to be accessed from any location.
```sql
$mysql -uroot -p
password
mysql> create database zabbix character set utf8 collate utf8_bin;
mysql> create user 'zabbix'@'<serverID>' identified with mysql_native_password by '<NewZabbixPassword>';
mysql> grant all privileges on zabbix.* to 'zabbix'@'<serverID>' with grant option;
mysql> quit;
```

- Import Zabbix Schema

> NOTE: The database for Server and Proxy are slightly different - use the correct zcat command based on the machine role the database will be used for!  Also, if multiple databases will exist on same server, each must use a unique name.

```bash
#NOTE: Because you didn't read the note above - The database for Server and Proxy are slightly different -
#      use the correct zcat command based on the machine role.
#ALSO NOTE: This does take a while in both cases. Please be patient and wait for the command to finish!

#Server Schema
$zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix

#Proxy Schema
$zcat /usr/share/doc/zabbix-proxy-mysql*/schema.sql.gz | mysql -uzabbix -p zabbix

$systemctl enable mysqld
```

- Setup Logrotate to prevent logs getting too large

Go to `vim /etc/logrotate.d/mysql` and verify the log location is correct, then uncomment as appropriate.

Do not forget to install the agent. See [Agent Configuration Guide](./Zabbix4AgentInstall.md) for details.

>Note that you may need to come back and add or adjust this after you install Application Server and front end Web Server so you can verify functionality

## Partitioning the Database (Server Database only) ##

> Skip down to NEXT STEPS if you do not need to partition your database

*Shout-out here to [ingus.vilnis](https://www.zabbix.com/forum/member/207631-ingus-vilnis), Senior Member on the Zabbix Forums who single-handedly provided all the information below based on the [YAMP wiki page](http://zabbix.org/wiki/Docs/howto/mysql_partitioning)*

Anytime you expect to hit somewhere above 500 NVPS (New Values Per Second) you should consider using partitioning instead of the built in housekeeper functions. In so doing, you sacrifice some historical granularity in favor of performance, so consider how important your historical data is before using this method of performance enhancements. In particular, with the housekeeper running, you can individually control how long you keep data for each item.With partitioning, however, you control how long you keep each item grouping (as defined by the table that is partitioned), instead of each individual item.

For my installations, I am using partitioning as there are no historical requirements defined for specific items and I am intending this installation to be capable of ~1500 NVPS or more. There are a couple different approaches and several different methods available for partitioning. I used the one highlighted and recommended in the [zabbix.org/wiki](http://zabbix.org/wiki/Docs/howto/mysql_partitioning) which is to partition by range (date ranges to be exact) using a perl script.

- Step one, verify there is no historical data that needs partitioned off

```sql
>SELECT FROM_UNIXTIME(MIN(clock)) FROM `history_uint`;
#Result should be:
#+---------------------------+
#| FROM_UNIXTIME(MIN(clock)) |
#+---------------------------+
#| NULL                      |
#+---------------------------+
#1 row in set (0.00 sec)
```

If it is not NULL, you can either truncate all tables or partition for each date between the history and today. To alter each table, do the Alter step for each table incrementing dates from the UNIXTIME to today.If you want to just truncate all data, do the following:

```sql
mysql>TRUNCATE TABLE `history`;
mysql>TRUNCATE TABLE `history_log`;
mysql>TRUNCATE TABLE `history_str`;
mysql>TRUNCATE TABLE `history_text`;
mysql>TRUNCATE TABLE `history_uint`;
mysql>TRUNCATE TABLE `trends_uint`;
mysql>TRUNCATE TABLE `trends`;
```

- Set the housekeeper table to use the BLACKHOLE database Engine

```sql
mysql>ALTER TABLE housekeeper ENGINE = BLACKHOLE;
```

- Create a separate system user in the database for running the script against

```sql
mysql>create user 'zbx_part'@'localhost' identified with mysql_native_password by '<NewZabbixPassword>';
mysql>GRANT SELECT, DELETE, DROP, ALTER ON zabbix.* TO 'zbx_part'@'localhost' with grant option;
```

- Alter each table so it is ready for partitioning

```sql
mysql>ALTER TABLE `history` PARTITION BY RANGE ( clock) (PARTITION p2019_06_06 VALUES LESS THAN (UNIX_TIMESTAMP("2019-06-07 00:00:00") div 1) ENGINE = InnoDB);
mysql>ALTER TABLE `history_log` PARTITION BY RANGE (clock) (PARTITION p2019_06_06 VALUES LESS THAN (UNIX_TIMESTAMP("2019-06-07 00:00:00") div 1) ENGINE = InnoDB);
mysql>ALTER TABLE `history_str` PARTITION BY RANGE (clock) (PARTITION p2019_06_06 VALUES LESS THAN (UNIX_TIMESTAMP("2019-06-07 00:00:00") div 1) ENGINE = InnoDB);
mysql>ALTER TABLE `history_text` PARTITION BY RANGE (clock) (PARTITION p2019_06_06 VALUES LESS THAN (UNIX_TIMESTAMP("2019-06-07 00:00:00") div 1) ENGINE = InnoDB);
mysql>ALTER TABLE `history_uint` PARTITION BY RANGE (clock) (PARTITION p2019_06_06 VALUES LESS THAN (UNIX_TIMESTAMP("2019-06-07 00:00:00") div 1) ENGINE = InnoDB);
mysql>ALTER TABLE `trends_uint` PARTITION BY RANGE ( clock) (PARTITION p2019_06 VALUES LESS THAN (UNIX_TIMESTAMP("2019-07-01 00:00:00") div 1) ENGINE = InnoDB);
mysql>ALTER TABLE `trends` PARTITION BY RANGE ( clock) (PARTITION p2019_06 VALUES LESS THAN (UNIX_TIMESTAMP("2019-07-01 00:00:00") div 1) ENGINE = InnoDB);
mysql>quit
```

- Install the Perl prerequisites

```bash
$yum -y install perl-CPAN perl-DateTime-TimeZone perl-DBD-MySQL perl-Sys-Syslog
```

- create the perl script in the /etc/zabbix/ directory

```bash
$vim /etc/zabbix/zabbix_partitioning.pl
```

- SCRIPT TEXT REMOVED BECAUSE IT DIDN'T WORK PROPERLY.  WATCH ZABBIX SUPPORT FORUMS FOR UPDATED SCRIPTS!

- Configure script to be run by root only (contains a database account password, so this helps protect it)
  
```bash
$chmod 700 /etc/zabbix/zabbix_partitioning.pl
```

- Finally, create the cron job to run the script

```bash
$vim /etc/cron.d/zabbix-mysql-partitioning

#CRON FILE ENTRY:

# Zabbix daily table partitioning
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
10 7 * * * root perl /etc/zabbix/zabbix_partitioning.pl 1>/var/log/zabbix/zabbix_partitioning.log  2>&1
```

---

## Next Steps ##

Now the Database is prepared.See my [Zabbix Application Server Installation Guide](./Zabbix4AppServerInstall.md) for instructions on setting up the Application Server.

---