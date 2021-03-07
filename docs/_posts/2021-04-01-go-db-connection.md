---
layout: post
title:  "MySQL Installation Notes"
date:   2021-04-01 00:00:00 +0800
categories: go database mysql
---
# Database Driver
Install a databse driver
e.g. for my sql
```
go get -u github.com/go-sql-driver/mysql
```

# Create Databse
Connect as root 
```
mysql.exe -u root -p
mysql> show databases;
mysql> create database ver1rev;
mysql>show create database ver1rev
mysql>use ver1rev
```

# Create Tables
```sql
CREATE TABLE IF NOT EXISTS users(
	user_id INT AUTO_INCREMENT PRIMARY KEY,
	username VARCHAR(255) NOT NULL,
	password VARCHAR(255) NOT NULL,
	role VARCHAR(255) NOT null
);
```

# Insert values.
```sql
insert into users values(1, "root", "password", "admin");
```

#Connect a Database Client
I am using Dbeaver
![DBeaver](/assets/images/2021-03-15-mysql-dbeaver.png)

[mysql-installer-windows]: <https://dev.mysql.com/downloads/installer/>