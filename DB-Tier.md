## Launch EC2 "t2.micro" Instance and In Sg, Open port "27017" for MongoDB
# DB Tier
## Install MYSQL DB
```
sudo yum update -y
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
sudo dnf install mysql80-community-release-el9-1.noarch.rpm -y
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
sudo dnf install mysql-community-client -y
sudo dnf install mysql-community-server -y
sudo systemctl start mysqld
sudo systemctl enable mysqld
sudo systemctl status mysqld
```

## Setup MYSQL DB

#### Allow any Host connect to DB
```
sudo vi /etc/my.cnf
```
ADD these Under [mysqld]
```
bind-address = 0.0.0.0
```
Restart MYSQL DB
```
sudo systemctl restart mysqld
```

Get your temporary root Password
```
sudo grep 'temporary password' /var/log/mysqld.log
```
Setup your root Password
```
sudo mysql_secure_installation
```
Login to your MYSQL
```
mysql -u root -p
```
Test it is working or Not
```
SELECT VERSION();
```
## Create our Application DB 'crud_app'
```
CREATE DATABASE IF NOT EXISTS crud_app;
```
Check the DB created or Not
```
SHOW DATABASES LIKE 'crud_app';
```
<img width="286" height="114" alt="image" src="https://github.com/user-attachments/assets/44822257-352a-4828-b9c5-d6c164d6c9b4" />

## Create one system User for our Application in DB
These user can login to DB to do Tasks
```
CREATE USER '<user-name>'@'Host-IP' IDENTIFIED BY 'Password-HERE';

GRANT ALL PRIVILEGES ON <DB-Name>.* TO '<user-name>'@'Host-IP';

FLUSH PRIVILEGES;
```
```
CREATE USER 'appuser'@'%' IDENTIFIED BY 'P@55Word';
GRANT ALL PRIVILEGES ON crud_app.* TO 'appuser'@'%';
FLUSH PRIVILEGES;
```
HERE % => any Host will connect
Switch to the database
```
USE crud_app;
```
Drop table if needed (optional safety cleanup)
```
DROP TABLE IF EXISTS users;
```
Create the `users` table with proper structure
```
CREATE TABLE IF NOT EXISTS users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255) NOT NULL UNIQUE,
  password VARCHAR(255) NOT NULL,
  role ENUM('admin', 'viewer') NOT NULL DEFAULT 'viewer',
  is_active TINYINT(1) DEFAULT 1,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Check the Grants of the "appuser" in DB

```
SHOW GRANTS FOR 'appuser'@'%';
```
<img width="443" height="130" alt="image" src="https://github.com/user-attachments/assets/9b78491c-4db2-4b7f-ab6d-aa331090c636" />