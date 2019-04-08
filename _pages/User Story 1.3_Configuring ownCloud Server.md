---
layout: default
title:  "user accounts"
permalink: /Configuration/
---

## Configuring ownCloud server

ownCloud requires a seperate database for storing administrative data. ownCloud currently supports the following databases:
* MySQL or MariaDB
*	PostgreSQL
*	Oracle (ownCloud Enterprise edition only) 

### Prerequisites for Configuring MySQL or MariaDB Database
Configuring MYSQL/MariaDb database requires you to install and set up the server software.

>**Note**: The choice of database will not alter the fuctionality of ownCloud. However, we reccomend using MySQL or MariaDB database engines for configuring ownCloud.

### MySQL or MariaDB with Binary Logging Enabled  

To avoid data loss under high load scenarios, ownCloud is currently using a `TRANSACTION_READ_COMMITTED` transaction isolation. This requires a disabled or correctly configured binary logging when using MySQL or MariaDB. Your system is affected if you see the following in your log file during the installation or update of ownCloud:  

    An unhandled exception has been thrown: exception `PDOException' with
    message `SQLSTATE[HY000]: General error: 1665 Cannot execute statement:
    impossible to write to binary log since BINLOG_FORMAT = STATEMENT and at
    least one table uses a storage engine limited to row-based logging.
    InnoDB is limited to row-logging when transaction isolation level is
    READ COMMITTED or READ UNCOMMITTED.'  
    
There are two solutions to rectify this problem:
1. Disable binary logging as it records all changes to your database, and the time for each change. The purpose of binary logging is to enable replication and to support backup operations.  

2. Change the BINLOG_FORMAT = STATEMENT in your database configuration file, or possibly in your database startup script, to BINLOG_FORMAT = MIXED or BINLOG_FORMAT = ROW. See [Overview of the Binary Log](https://mariadb.com/kb/en/library/overview-of-the-binary-log/) and [The Binary Log](https://dev.mysql.com/doc/refman/5.6/en/binary-log.html) for detailed information.  

### MySQL / MariaDB **READ COMMITED** Transaction Isolation Level

As discussed, ownCloud is using the `TRANSACTION_READ_COMMITTED` transaction isolation level to avoid data loss under high load scenarios (e.g., by using the sync client with many clients/users and many parallel operations). In this case, you need to configure the transaction isolation level accordingly. Please refer to the [MySQL Reference Manual](https://dev.mysql.com/doc/refman/5.7/en/set-transaction.html) for detailed information.  

### MySQL or MariaDB Storage Engine  

InnoDB is supported as a storage engine on ownCloud 7. There are some shared hosts who do not support InnoDB and only MyISAM and so running ownCloud on such an environment is not supported.  

### Parameters  

Follow the instructions in [The Installation Wizard](https://doc.owncloud.org/server/10.1/admin_manual/installation/installation_wizard.html) for setting up ownCloud for using different database engines. You do not have to edit the respective values in the config/config.php. However, in special cases (for example, for connecting your ownCloud instance to a database created by a previous installation of ownCloud), some modification might be required.

### MySQL or MariaDB Database Configuration 

Ensure the following before you configure MySQL or MariaDB database:
* Installing and enabling the `pdo_mysql` extension in PHP
* If the database runs on the same server as ownCloud, **mysql.default_socket** points to the correct socket.   

MariaDB is backwards compatible with MySQL. So you will not need to replace or revise any, existing, MySQL client commands.  

This is how a PHP configuration in /etc/php5/conf.d/mysql.ini could look like:

    # configuration for PHP MySQL module
    extension=pdo_mysql.so

    [mysql]
    mysql.allow_local_infile=On
    mysql.allow_persistent=On
    mysql.cache_size=2000
    mysql.max_persistent=-1
    mysql.max_links=-1
    mysql.default_port=
    mysql.default_socket=/var/lib/mysql/mysql.sock  # Debian squeeze: /var/run/mysqld/mysqld.sock
    mysql.default_host=
    mysql.default_user=
    mysql.default_password=
    mysql.connect_timeout=60
    mysql.trace_mode=Off  
    
To create database tables, you first need to create a database user by using the MySQL command line interface. The database tables will be created by ownCloud when you login for the first time.  

To get started, log into MySQL with the administrative account and use the following command line:  

    mysql -uroot -p  

which will give a **mysql>** or **MariaDB [root]>** command prompt. Now enter the following lines and confirm them by clicking the enter key:

    CREATE DATABASE IF NOT EXISTS owncloud;
    GRANT ALL PRIVILEGES ON owncloud.* TO 'username'@'localhost' IDENTIFIED BY 'password';  
    
Now you can quit the prompt by entering:

    quit    
    
An ownCloud instance configured with MySQL would contain the hostname on which the database is running, a valid username and password to access it, and the name of the database. The `config/config.php` as created by the installation wizard would therefore contain entries like this:  

    <?php

      "dbtype"        => "mysql",  
      "dbname"        => "owncloud",  
      "dbuser"        => "username",  
      "dbpassword"    => "password",  
      "dbhost"        => "localhost",  
      "dbtableprefix" => "oc_",  
      
For supporting additional features such as emoji, both MySQL and ownCloud needs to be configured to use [4-byte Unicode Support](https://doc.owncloud.org/server/10.1/admin_manual/configuration/database/linux_database_configuration.html#configure-mysql-for-4-byte-unicode-support).   

