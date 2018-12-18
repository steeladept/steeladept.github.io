---
title: Zabbix Automation - AutoRegistration
keywords: zabbix
last_updated: Dec 18, 2018
tags: [zabbix]
summary: "Zabbix automation allows for many different actions at different times.  One of the first ones you should consider implementing, even before adding servers to monitor, is the Auto Registration automation.  This allows a device to automatically get the templates and host group assignments based on predefined characteristics, minimizing work needed to add a new device into monitoring"
sidebar: zabbix_sidebar
permalink: zabbixAdmin.html
folder: zabbix
---

## Auto-registration ##

Zabbix auto-registration is a predefined action that Zabbix should take with any newly identified device.  It is an action or series of actions Zabbix does based on matching predefined conditions related to the Host Name, any defined Host Metadata, or the Proxy the Host is connected through. It is most often used to identify characteristics about a machine and assign it to the appropriate Host Group(s) and/or assign the appropriate template(s) for monitoring that machine.  

>Auto-registration actions are NOT cumulative, so you do not create one for each OS, and another for each application, and another for each location, and layer them.  Instead, you must fully define the machine to match only one auto-registration action.  Any changes to this base must be made after the machine is registered.

### Create an auto-registration Action ###

To create an auto-registration action, first navigate to *Configuration>Actions*; then, in the Event Source dropdown, change the selection to Auto registration.

---