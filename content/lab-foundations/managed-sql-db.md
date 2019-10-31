---
title: "Create a MySQL Relational DB in minutes"
date: 2019-03-29T17:59:04+03:00
weight: 3
---


## Objectives

In this tutorial, you will learn how to create an environment to run your MySQL database, connect to the database, and delete the DB instance.  We will do this using Amazon Relational Database Service (Amazon RDS).  Amazon RDS makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while managing time-consuming database administration tasks, freeing you up to focus on your applications and business. 


## Services Used in this Lab:

- *Amazon Relational Database Service (Amazon RDS):* AWS RDS makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while automating time-consuming administration tasks such as hardware provisioning, database setup, patching and backups. It frees you to focus on your applications so you can give them the fast performance, high availability, security and compatibility they need. Amazon RDS is available on several database instance types - optimized for memory, performance or I/O - and provides you with six familiar database engines to choose from, including `Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle Database`, and `Microsoft SQL Server`. https://aws.amazon.com/rds/ 
- *AWS Database Migration Service:* You can use  AWS DMS to easily migrate or replicate your existing databases to Amazon RDS.  AWS Database Migration Service helps you migrate databases to AWS quickly and securely. The source database remains fully operational during the migration, minimizing downtime to applications that rely on the database. The AWS Database Migration Service can migrate your data to and from most widely used commercial and open-source databases. AWS Database Migration Service supports homogenous migrations such as `Oracle to Oracle`, as well as heterogeneous migrations between different database platforms, such as `Oracle or Microsoft SQL Server to Amazon Aurora`. With AWS Database Migration Service, you can continuously replicate your data with high availability and consolidate databases into a petabyte-scale data warehouse by streaming data to Amazon Redshift and Amazon S3. Learn more about the supported source and target databases.  https://aws.amazon.com/dms/ 

## Lab Prerequisites

None

## Expected Costs

Everything done in this tutorial is free-tier eligible. We will use Amazon RDS to create a MySQL DB Instance with db.t2.micro DB instance class, 5 GB of storage, and automated backups enabled with a retention period of one day.

## Lab Steps

This lab is based on the tutorial: https://aws.amazon.com/getting-started/tutorials/create-mysql-db/ We won't repeat the steps in this guide.  

1. Do the Step 1 & Step 4 in the tutorial link above (We won't spend time connecting to the DB)
1. Let's spend a few minutes to the Read the blog article to understand HA for SQL DB's : https://aws.amazon.com/blogs/database/amazon-rds-under-the-hood-multi-az/ 
   - Understanding High Availability
      - Creating SQL DBs is an art and a science. Creating SQL DB's to be used in business critical data is like black magic if you are not a DB admin veteran. 
      - Amazon Web Services (AWS) customers bet their businesses on their data store and highly available access to it. For these customers, Multi-AZ configurations provide an easy-to-use solution for high availability (HA).
1. Let's discuss a few advanced config options for securing, tuning your DB: 

- Network & Security:
   - Subnet Group: Each DB subnet group should have subnets in at least two Availability Zones in a given region. 
- Database Options
   - DB Parameter Group: You manage your DB engine configuration by associating your DB instances with parameter groups. 

        ```
        ={log(DBInstanceClassMemory/8187281418)*1000}
        CHARACTER SET character_set_name COLLATE collation
        ```	

    - Option Group: Used for instance for MS SQL Native Backup and Restore
- Backup
   - Backup Retention Period: For this tutorial, set this value to 1. But in your production system, you can choose the number of days to retain the backup you take. 
- Monitoring
   - Enable Enhanced Monitoring: Use the default of *No* to stay within the free tier. But in your production system, enabling Enhanced Monitoring will give you metrics in real time for the operating system (OS) that your DB instance runs on.
-Maintenance
   - Auto Minor Version Upgrade: Select Yes to receive automatic updates when they become available.
   - Maintenance Window: Select No Preference.

## Summary

Congratulations! You have created, connected to, and deleted a MySQL Database Instance with Amazon RDS.  Amazon RDS makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while managing time-consuming database administration tasks, freeing you up to focus on your applications and business.

## Deleting Lab Resources

Make sure you delete RDS instance as described in the Tutorial Step 4.
