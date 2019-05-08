---
title:  "MySQL Database Tips"
published: true
permalink: mysqltipspost1.html
summary: "Learning Zabbix really means learning the database behind Zabbix as much as it does the application iteslf.  This is an initial post of a few MySQL Queries I found
critical to my maintenance of Zabbix."
tags: [zabbix, troubleshooting]
---

![alt text:  Learn Banner][learn]

I am quickly learning that knowing Zabbix means knowing the underlying database as much as it does the application itself. This is a quick and simple post of several MySQL Database Queries I seem to need frequently while maintaining Zabbix.  Most of the time, I need it for maintaining Proxy servers, so much of this will probably need edited slightly if it is for the main database.

1) Find the largest tables:

```sql
    SELECT table_schema as `Database`,
    table_name AS `Table`,
    round(((data_length + index_length) / 1024 / 1024),2) `Size in MB`  
    FROM information_schema.TABLES
    WHERE table_schema='zabbix'
    ORDER BY `Size in MB`;
```

>Result is a table of all tables in the Database with the largest at the bottom *Use DESC to put it at the top*. (Drop the WHERE clause to see across all databases)

2) Select the top 10 values of a table:

```sql
    SELECT * FROM proxy_history LIMIT 10;
```

3) Converting Unixtime to date:

Zabbix uses UNIXTIME in their 'clock' field of most tables that time is relevant for (such as history tables).  This useful utility will help you convert that time to human readable times.

```sql
    SELECT from_unixtime(1557153218);
```

>Result is a single row view with the datetime equivalent.

4) Truncate a table:

```sql
    TRUNCATE TABLE proxy_history;
```

NOTE:  This frequently does not work due to foreign key dependencies.  

5) Deleting specific records in a table:

```sql
    DELETE ALL
    FROM proxy_history
    WHERE 'clock'>1557153218;
```

>Result is all entries are deleted from the table if they are older than the unixtime listed.  (Remove the where clause to delete all entries from the table - equivalent to a truncate command but slower)

---

[learn]:  images/Banners/learnbanner.png "Learn Banner"