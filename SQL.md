- [1. Install MySQL server](#1-install-mysql-server)
- [2. Execute the file](#2-execute-the-file)
- [3. Install phpMyAdmin](#3-install-phpmyadmin)
- [4. Basic Commands](#4-basic-commands)
- [5. Execute one SQL file](#5-execute-one-sql-file)
- [6. Install MySQL Workbench](#6-install-mysql-workbench)
- [7. Install DBeaver](#7-install-dbeaver)
- [8. Reset MySQL Root password](#8-reset-mysql-root-password)
- [9. Create New MySQL user](#9-create-new-mysql-user)

## 1. Install MySQL server

```sh
sudo apt update
sudo apt install mysql-server
sudo mysql_secure_installation
sudo mysql
```

## 2. Execute the file

```sh
sudo mysql
mysql -u username -p
source filename.sql;
```

## 3. Install phpMyAdmin
```sh
sudo apt list --installed | grep phpmyadmin
sudo apt update
sudo apt install phpmyadmin
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
sudo systemctl restart apache2
sudo systemctl status apache2
sudo systemctl start apache2
sudo nano /etc/apache2/conf-available/phpmyadmin.conf
sudo a2enconf phpmyadmin
sudo systemctl restart apache2
```

## 4. Basic Commands

```sh
SHOW DATABASES;
CREATE DATABASE database_name;
USE database_name;
SHOW TABLES;
DESCRIBE table_name;
CREATE TABLE table_name (
    id INT AUTO_INCREMENT PRIMARY KEY,
    column_name1 VARCHAR(255),
    column_name2 INT
);
INSERT INTO table_name (column_name1, column_name2) VALUES ('value1', value2);
SELECT * FROM table_name;
UPDATE table_name SET column_name1 = 'new_value' WHERE condition;
DELETE FROM table_name WHERE condition;
DROP TABLE table_name;
SELECT USER();
SELECT VERSION();
EXIT;
```

## 5. Execute one SQL file

```sh
sudo mysql
USE database_name;
SOURCE /path/to/your/file.sql;
```

## 6. Install MySQL Workbench

```sh
sudo apt update
sudo apt install mysql-workbench
mysql-workbench
```

```sh
wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.22-1_all.deb
sudo apt update
sudo apt install mysql-workbench
mysql-workbench --version
```

## 7. Install DBeaver

```sh
sudo snap install dbeaver-ce

# Handle Public Key Retrieval issue
#jdbc:mysql://localhost:3306/your_database?allowPublicKeyRetrieval=true&useSSL=false
```

## 8. Reset MySQL Root password

```sh
sudo systemctl stop mysql
sudo mysqld_safe --skip-grant-tables &
mysql -u root

USE mysql;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
FLUSH PRIVILEGES;
EXIT;

sudo systemctl stop mysql
sudo systemctl start mysql

GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY 'your_password' WITH GRANT OPTION;
FLUSH PRIVILEGES;

SELECT User, Host FROM mysql.user WHERE User = 'root';
CREATE USER 'root'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_password';
FLUSH PRIVILEGES;
```

```sh
sudo tail -f /var/log/mysql/error.log
```

## 9. Create New MySQL user

```sh
sudo mysql -u root -p
sudo mysql
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON benedetta.* TO 'newuser'@'localhost';
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost' WITH GRANT OPTION;
GRANT SELECT, INSERT, UPDATE, DELETE ON benedetta.* TO 'newuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
mysql -u newuser -p
```

```sh
CREATE USER 'benedetta_user'@'localhost' IDENTIFIED BY 'strongpassword';
GRANT ALL PRIVILEGES ON benedetta.* TO 'benedetta_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```