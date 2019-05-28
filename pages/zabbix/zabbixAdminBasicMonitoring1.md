---
title: Zabbix Administration - Basic Monitoring
keywords: zabbix
last_updated: May 20, 2019
tags: [getting_started, zabbix]
summary: "Zabbix can monitor many different things many different ways. This section deals with monitoring basic compute information: CPU, Disk Space, Memory, etc."
sidebar: zabbix_sidebar
permalink: zabbixAdminBasic1.html
folder: zabbix
---

## Zabbix Administration - Basic Hardware Monitoring ##

As already referenced, Zabbix was built for basic server monitoring.  This "basic" monitoring includes monitoring CPU, memory, and disk space resources to ensure your applications have enough to do the work efficiently. It can also be used to help identify over-provisioned systems and allows you to make smarter decisions on where and how to run your machines efficiently. In my environment, I use these monitors to identify memory bottlenecks and to minimize or prevent system shutdown due to running out of disk space. While the application setup plays a big part in preventing this, using Zabbix to monitor helps in case the application design fails and/or goes beyond design parameters.

### CPU Resources ###

---