---
title: Creating a CENTOS 7 Base Image
keywords: linux
last_updated: Mar 26, 2019
tags: [getting_started, linux]
summary: "Creating a CENTOS 7 Base Image - source"
sidebar: linux_sidebar
permalink: centos7base.html
folder: linux
---

Whenever I go into a new environment, it seems they always have a good image for Windows, if used, and a good image for older/other versions of Linux.  However, at the last 3 companies I worked at, for a variety of reasons, they never had a good CENTOS 7 base image.  Each time I created it, and yet each time I used a different source to determine how to create it. So now I am codifying it in my own Blog/Notes so I know where to go to get consistent results.

###Initial setup
1. Download and install the OS (I use the latest build of CENTOS 7 Minimal Install).  This should include basic network setup using 
2. Update the OS to current (yum -y update)
3. Edit firewall rules (firewalld) to allow common services (NTP, SSH, HTTP, HTTPS, etc.)  
*NOTE - HTTP and HTTPS are needed for updates.  Do not remove unless you do not ever plan on updating servers*
    a. firewall-cmd --list-all (to see what is already installed and may need removed)
    b. firewall-cmd --permanent --add-services=ssh, ntp, http, https
4. Add Utilities (Optional, but often required by applications)
	a. **WGET** (yum -y install wget)
	b. **EPEL** Repository (yum -y install epel-release)
	c. **ACK** (yum -y install ack)
	d. **VIM** (yum -y install vim)
	e. **Policy Core Utilities** (yum -y install policycoreutils-python)
5. Configure VMware Tools (if needed)
	a. Add the VMware repository and prerequisites (yum -y install open-vm-tools)
    b. reboot server
6. Configure NTP (yum -y install ntp)
	a. Set Expedient as NTP server  (/usr/sbin/ntpdate pool.ntp.org)
	b. Start NTP server (systemctl start ntpd.service)

IF you have an AD Domain and wish to add the computer to the domain, there are some things you can prep for before deployment:

7. Install samba-winbind and pam_krb5 packages (yum -y install samba-winbind  pam_krb5)
	a. Update Authconfig (see authConfig below)
8. Disable Root SSH Login
    a. Edit the /etc/ssh/sshd_config to change "PermitRootLogin" entry from "yes" to "no"
9. Verify OS seems to be working as expected before cleanup

###WindBind Authconfig configuration command

authconfig \
--enablekrb5 \
--krb5kdc="adserver1.company.com adserver2.company.com" \
--krb5realm="DOMAIN.COM" \
--enablewinbind \
--enablewinbindauth \
--smbsecurity=ads \
--smbworkgroup="OU_NAME" \
--smbrealm="DOMAIN.COM" \
--smbservers="adserver1.company.com adserver2.company.com" \
--winbindtemplatehomedir=/home/%U \
--winbindtemplateshell=/bin/bash \
--enablemkhomedir \
--enablewinbindusedefaultdomain \
--enablelocauthorize \
--update

###"SYSPREP" Image

Clean-up all packages and network rules [footnote 1](1)
1. yum clean all
2. package-cleanup –oldkernels –count=1
3. stop rsyslog and auditd services
4. logrotate -f /etc/logrotate.conf
5. rm -f /var/log/*-???????? /var/log/*.gz
6. rm -f /var/log/dmesg.old
7. rm -rf /var/log/anaconda
8. cat /dev/null > /var/log/audit/audit.log
9. cat /dev/null > /var/log/wtmp
10. cat /dev/null > /var/log/lastlog
11. cat /dev/null > /var/log/grubby
12. rm -f /etc/udev/rules.d/70*
13. sed –i”.bak” ‘/UUID/d’ /etc/sysconfig/network-scripts/<active NIC>
    *NOTE - Repeat for each active NIC being used - typically it is one on a gold image*
14. rm -f `/etc/ssh/*key*`
15. rm -f ~root/.bash_history
16. unset HISTFILE
17. rm -rf ~root/.ssh/
18. history -c
19. sys-unconfig
20. Shutdown image or convert to template if in VMWare

###New Machine Setup
1. Start new machine from template or image copy
2. Setup network settings using nmtui command
IF WINDBIND IS USED: 
3. Add machine to the domain (net ads join <domain> -U <domain admin ID>)
4. Start the winbind service (systemctl start/enable windbindd.service)

---
[1]: (http://everything-virtual.com/2016/05/06/creating-a-centos-7-2-vmware-gold-template/)  "Creating a CENTOS 7.2 Gold Template"