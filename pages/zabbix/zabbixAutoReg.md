---
title: Zabbix Automation - AutoRegistration
keywords: zabbix
last_updated: Dec 18, 2018
tags: [zabbix]
summary: "Zabbix automation allows for many different actions at different times.One of the first ones you should consider implementing, even before adding servers to monitor, is the Auto Registration automation.This allows a device to automatically get the templates and host group assignments based on predefined characteristics, minimizing work needed to add a new device into monitoring"
sidebar: zabbix_sidebar
permalink: zabbixautoreg.html
folder: zabbix
---

## Auto-registration ##

Zabbix auto-registration is a predefined action that Zabbix should take with any newly identified device.It is an action or series of actions Zabbix does based on matching predefined conditions related to the Host Name, any defined Host Metadata, or the Proxy the Host is connected through. It is most often used to identify characteristics about a machine and assign it to the appropriate Host Group(s) and/or assign the appropriate template(s) for monitoring that machine.

>Auto-registration actions are NOT cumulative, so you do not create one for each OS, and another for each application, and another for each location, and layer them.Instead, you must fully define the machine to match only one auto-registration action.Any changes to this base must be made after the machine is registered.

### Create an auto-registration Action ###

To create an auto-registration action, first navigate to *Configuration>Actions*; then, in the Event Source dropdown, change the selection to "Auto registration".

![alt text: Zabbix Auto-Registration][ZabbixAutoReg]

Press the Create Action button, and then on the Actions screen give the Action a descriptive name.

Next, you need to choose your condition.I tend to use Host Metadata because I define that in the Agent and I don't have to rely on others to follow a naming standard.This is particularly important when you run into a situation like I did recently where one company buys another, and the standards are not the same (or in one case, didn't exist).However, if you are able to rely on a naming standard, Host Name is very likely a good choice as well - especially if you can use both to further enhance your Auto-registration granularity.

Once you have your condition, press the *Add* link (I always forget this and press the Add Button, which does nothing because no condition was added yet!).If you have additional conditions, you can add them as well.When done, press the `Add` button.

The key to making all this work is the next step - Click on the Operations Link near the top of the form.I leave the notification information alone, though you could always modify it to meet your needs.The key part is the Operations section.Press the *New* link.As with conditions, you can add several operations to this page.The first one I will walk through is Adding it to a Host Group.

In the Operations details, change the Operation type to Add to host group, then in the Host groups line, add as many appropriate Host Groups as you need to.

![alt text: Auto-Registration Add Host Groups][AutoRegAddHost]

Next, add another operation and change the type to Link to Template. Here, you want to apply all appropriate templates for this type of machine.

Finally, if you want Zabbix to discover running services and add them to monitoring automatically, add a third operation for *Set host inventory mode* and set it to Automatic.

![alt text: Auto-Registration Operations Completed][AutoRegOpsComplete]

There are other options for you to discover and configure if appropriate for your situation, however, I do not use the others at this time.

---

[ZabbixAutoReg]: images/Zabbix/autoregister.png "Zabbix Auto-Registration"
[AutoRegAddHost]: images/Zabbix/araddhosts.png "Auto-Registration Add Host Groups"
[AutoRegOpsComplete]: images/Zabbix/aropscomp.png "Auto-Registration Operations Completed"