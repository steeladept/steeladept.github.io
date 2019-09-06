---
title: Zabbix Administration - Tips, Tricks, and Insights
keywords: zabbix
last_updated: May 20, 2019
tags: [getting_started, zabbix]
summary: "Zabbix is a huge monitoring solution that is capable of MANY different ways of monitoring. Setting it up efficiently has as much to do with what and how you intend to monitor things as it does with how you implement it. This section includes tips, tricks, and insights on how to manage and scale a Zabbix installation."
sidebar: zabbix_sidebar
permalink: zabbixAdminTTI-Setup.html
folder: zabbix
---

## Tips, Tricks, and Insights - Setting Zabbix up for Success ##

Zabbix can be configured in MANY different ways to accomplish MANY different goals. How you set it up will define how flexible the system is, how efficient it does it's job, and how well it achieves the goal you have. Still there are many basic truths of how to best setup Zabbix for a fully flexible and scalable design.

### Configuration Files ###

One trick I learned early on is to use IP addresses in the configuration files - even if it is localhost. There are a couple reasons for this:

1) If your DNS isn't rock solid, it can break monitoring right when you need it most.
2) Some entries do not seem to work with DNS entries regardless of using either short name or FQDN.
3) The biggest reason, however, is for scalability.

1 & 2 are self-explanatory, but let's take a second to understand why I say this enhances scalability. Most people start out with Zabbix in a test environment, or at least at very small scale. Perhaps they use the appliance, or perhaps they just don't need to separate the various components of the system for the test. Either way, all components are in a single server. Unfortunately test environments far too often become production environments when they prove useful - no time or allowance is provided to go back and properly architect the solution. The trick is to setup the test environment to be a production environment from the outset, and that means scalability.

So now you see the problem, but that still doesn't explain how using IP addresses in configuration files helps. The key piece here is that it is far easier to grep specific IP addresses and find all locations that need changed in the database configuration file, for example, if you reference them by IP. Some are intrinsic to the database, and those can stay pointed to "localhost", but where the database points to the application server, or the frontend server, or even a proxy server, those should all be IP addresses (even if they are all the same address). As you scale out, you can change the IP addresses for each as appropriate without interrupting anything else in your environment. And if you are doing a complete server refresh with new IP addresses, it is far easier to change those few than it is to go through every file to see if it is involved.

It is a minor tip, to be sure, but I found it saved me quite a bit of time as a Zabbix administrator, especially as the environment grew.

### Order of Operations ###

It is a LOT easier to stand up a new instance of Zabbix if you do it in a specific way. Even after installation, the order you build things matters. The first step you must do is create a Host Group as that is how you will apply many of your templates and how you will group servers for quicker access. Then you create the templates. This is a whole section to itself, but is generally we documented, so I will leave that to a later page. Next is hosts, that is, unless you take my advice and work out auto-registration (which is under actions).  Next you create any automation actions. Finally, after all that is done, do you start concerning yourself with Users, User groups, etc.

* Create Host Groups
  * By Operation System (this is where you apply a base template)
  * By Application (this is where you assign your application templates)
  * By Location (if desired)
  * By Department (if desired)
  * By Patching Cycle (if desired)
  * Etc.
* Create templates
  * Start by copying an existing starter template
  * Define known items you want to monitor that are discovered by LLD (example c:\ drive)
  * Create triggers for new items
  * Define priorities for new triggers
  * Set priority for unknown/undefined LLD monitors (I usually set it lower as they are not as critical if they are not known and predefined)
* Create Auto-registration and Auto-discovery Actions (if any)
* Setup Administrative tasks (Users, User Groups, etc.)
  
Other tasks, such as setting up trigger actions, proxy setups, etc. can be done in more or less any order, and will not be any harder or easier by doing after all the above is done.  However, it is easier to setup your agent configuration files if you setup and know your proxy settings before configuring the servers that will be monitored by them.

### Template Creation ###

Speaking of templates, there are some tricks to making template creation a lot easier as well.

* Don't recreate existing templates! - You can copy them into new ones, but don't create them from scratch. It unnecessarily uses braincells that can be used for the new items.
* Create the template items in order.  You can skip over categories you don't need to create new or won't create at all but go in order:
  * Application
  * Item
  * Trigger
  * Graphs
  * Discovery
  * Web

### Useful MySQL Scripts ###

* Determine overall size of database

```sql
SELECT table_schema "zabbix", sum(data_length + index_length)/1024/1024/1024 "DВ size in GB" FROM information_schema.TABLES GROUP BY table_schema;
```

* See the top 20 Largest tables in the database
  
```sql
SELECT table_name, table_rows, data_length, index_length, round(((data_length + index_length) / 1024 / 1024 / 1024),2) "Size in GB" FROM information_schema.tables WHERE table_schema = "zabbix" order by round(((data_length + index_length) / 1024 / 1024 / 1024),2) DESC LIMIT 20;
```

---
