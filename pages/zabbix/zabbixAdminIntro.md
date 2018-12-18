---
title: Zabbix Administration
keywords: zabbix
last_updated: Dec 18, 2018
tags: [getting_started, zabbix]
summary: "Zabbix is a huge monitoring solution that is capable of MANY different ways of monitoring.  Further, it has the capability of being extended through a plug-in architecture to allow for future technologies monitoring as they evolve and grow"
sidebar: zabbix_sidebar
permalink: zabbixAdmin.html
folder: zabbix
---

## Zabbix Administration ##

Zabbix can be monitored many different ways and each installation reflects that.  What is monitored, how frequently, and how it responds to an alert condition are all unique between each installation.  However, just because each installation is different doesn't mean we need to reinvent the wheel each time.  Though the same template pattern used for installation and setup, we are able to granularly control these settings at the individual alert level, if necessary, while maintaining a system-wide standard that works for the organization.

Describing how to administer a monitoring solution is a bit like describing how to care for a child - each is different, and no matter how much similarities there are, it will never work for you quite the same as it did for me. So instead, I will describe different situations, and how I approached them, with an explanation of why I approached it that way to describe one way of accomplishing the goal. If that works for you, great!  If it doesn't, please leave a note on your approach so all the readers can learn alternative approaches to an issue. I am hardly a Zabbix expert, but I do know my environment and what I want to learn about it; so this is where I will start.  But before we begin, here is a brief overview of the areas where Zabbix has capabilities that we will discuss in the rest of the documentation:

### Network Monitoring ###

The right hand of Zabbix, Network monitoring is similar to Server monitoring but for network machines. Switches, Routers, and other similar dedicated network equipment is of critical importance to any connected organization, and Zabbix was made to monitor this hardware as much as the servers themselves. It was designed to keep track of not only the equipment, but the connectivity between components their ability to communicate over key ports/protocols and with some advanced monitoring call "flow" monitoring, can even describe attributes associated with the communication packets themselves.

### Basic Hardware Monitoring ###

The left hand of Zabbix, Server monitoring, or hardware monitoring includes monitoring CPU, memory, and disk space to ensure your applications have enough resources to do the work efficiently.  It can also be used to help identify over-provisioned systems and allows you to make smarter decisions on where and how to run your machines efficiently. This is a core function of Zabbix. In my environment, I use these monitors to identify memory bottlenecks and to minimize or prevent system shutdown due to running out of disk space. While the application setup plays a big part in preventing this, using Zabbix to monitor helps in case the application design fails and/or goes beyond design parameters.

### Service Monitoring ###

Going a step beyond the hardware, service monitoring is a feature of Zabbix that allows it to keep track of and report on the status of key services. HTTP services for websites, for example, are quite critical to their function, and being able to alarm if it fails means that the website can be brought up that much faster.  The key to effective use of Service monitoring is identifying 1) what services are truly key services, and 2) knowing (and ideally monitoring for) service dependencies.

### Site Monitoring ###

A step beyond most traditional network monitoring solutions and venturing into the realm of Application Performance Monitoring (APM) is site monitoring. This gray area can be used to verify a website or application is up and running AND ACCESSIBLE to users. Zabbix accomplishes this through a scripted walk through the site/application - similar to most synthetic user monitoring solutions.

### Event Correlation ###

Zabbix was designed with all these tools, but often one issue causes a cascade of effects across many of these different areas. For example, if one switch goes down, not only does the Network Monitoring piece pick up that outage, but also all downstream systems that loose connectivity because of it. Likewise any connected servers would go dark; and if they were web servers, it may well cause sites to appear down. Using event correlation rules allows Zabbix to automatically identify the hierarchy of errors and suppress notification of errors due to a higher level error.  This speeds root cause analysis and resolution minimizing downtime and personnel troubleshooting time.

### Task Automation ###

Thanks to an included API, Zabbix can be fully integrated and automated with a variety of scripts and/or 3rd party tools.  Two way integration with ticketing systems, Configuration Management Databases, and a host of other tooling solutions are completely within the realm of possibility with Zabbix. Even internally, Zabbix has built a great deal of automation into the discovery and registration process, allowing (for example) users to install the agent as part of their automated provisioning process and it automatically is added to Zabbix Monitoring without additional administrative overhead.

---