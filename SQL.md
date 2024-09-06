- [1. Install MySQL server](#1-install-mysql-server)
- [2. Execute the file](#2-execute-the-file)
- [3. Install phpMyAdmin](#3-install-phpmyadmin)
- [4. Basic Commands](#4-basic-commands)
- [5. Execute one SQL file](#5-execute-one-sql-file)
- [6. Install MySQL Workbench](#6-install-mysql-workbench)
- [7. Install DBeaver](#7-install-dbeaver)
- [8. Reset MySQL Root password](#8-reset-mysql-root-password)
- [9. Create New MySQL user](#9-create-new-mysql-user)
- [10. Common Commands to use](#10-common-commands-to-use)
- [11. Sample Commands for JOINS](#11-sample-commands-for-joins)
  - [Create Database](#create-database)
  - [Create Table](#create-table)
  - [Data Insertion](#data-insertion)
  - [Inner Join](#inner-join)
  - [Left Join](#left-join)
  - [Right Join](#right-join)
  - [Full Outer Join](#full-outer-join)
  - [Self Join](#self-join)
  - [Cross Joins](#cross-joins)
  - [Complex Queries](#complex-queries)
- [12. Advanced Commands for JOINS](#12-advanced-commands-for-joins)
  - [Sort Top Employees](#sort-top-employees)
  - [Rank in departments](#rank-in-departments)
  - [Employees working on multiple projects](#employees-working-on-multiple-projects)
  - [Average Salary](#average-salary)
  - [Employees not working on any projects](#employees-not-working-on-any-projects)
  - [Employees working on same department as Alice](#employees-working-on-same-department-as-alice)
  - [Correlated Subquery: Find Employees Earning More Than the Average Salary in Their Department](#correlated-subquery-find-employees-earning-more-than-the-average-salary-in-their-department)
  - [Common Table Expressions (CTE) to List Employees and Their Project Count](#common-table-expressions-cte-to-list-employees-and-their-project-count)
  - [Subquery with Joins: Find Departments Where All Employees Earn More Than 6000](#subquery-with-joins-find-departments-where-all-employees-earn-more-than-6000)
  - [Advanced JOIN and Aggregate: Find the Project with the Highest Average Employee Salary](#advanced-join-and-aggregate-find-the-project-with-the-highest-average-employee-salary)
  - [Join Across Multiple Tables: List Employees, Their Departments, and Projects](#join-across-multiple-tables-list-employees-their-departments-and-projects)
  - [Nested Queries with Joins: Find the Department with the Highest Total Salary](#nested-queries-with-joins-find-the-department-with-the-highest-total-salary)
  - [Cleanup function](#cleanup-function)
- [13. More Advanced Queries for Joins](#13-more-advanced-queries-for-joins)
  - [Recursive Common Table Expression (CTE)](#recursive-common-table-expression-cte)
  - [JSON Data Handling](#json-data-handling)
  - [Window Functions: Running Total](#window-functions-running-total)
  - [Advanced Grouping: GROUPING SETS, CUBE, and ROLLUP](#advanced-grouping-grouping-sets-cube-and-rollup)
  - [Lateral Join (Correlated Subquery in FROM)](#lateral-join-correlated-subquery-in-from)
  - [Dynamic Pivoting](#dynamic-pivoting)
  - [Advanced Filtering: Filtering Based on Rank](#advanced-filtering-filtering-based-on-rank)
  - [Advanced Subquery with EXISTS](#advanced-subquery-with-exists)
  - [Advanced Aggregate Functions: Percentile Calculations](#advanced-aggregate-functions-percentile-calculations)
  - [Common Table Expressions (CTEs) with Recursion for Date Ranges](#common-table-expressions-ctes-with-recursion-for-date-ranges)
  - [Full Text Search](#full-text-search)
  - [Multi-Table Update](#multi-table-update)
  - [Stored Procedures with Dynamic SQL](#stored-procedures-with-dynamic-sql)
  - [Conditional Aggregates with CASE](#conditional-aggregates-with-case)
  - [Query Optimization: Using Indexes](#query-optimization-using-indexes)

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

## 10. Common Commands to use

```sh
SELECT * FROM admin WHERE name LIKE 't%';
SELECT * FROM admin WHERE name LIKE '%t%';
SELECT * FROM admin WHERE name LIKE 'ess%' OR email LIKE 't%'
```

## 11. Sample Commands for JOINS

### Create Database
```sh
CREATE DATABASE testdb;
USE testdb;
```

### Create Table
```sh
-- Create Employees Table
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50),
    dept_id INT,
    salary DECIMAL(10,2)
);

-- Create Departments Table
CREATE TABLE departments (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50)
);

-- Create Projects Table
CREATE TABLE projects (
    project_id INT PRIMARY KEY,
    project_name VARCHAR(50),
    emp_id INT
);
```

### Data Insertion
```sh
-- Insert Data into Employees
INSERT INTO employees (emp_id, emp_name, dept_id, salary) VALUES
(1, 'Alice', 1, 6000.00),
(2, 'Bob', 2, 7000.00),
(3, 'Charlie', 1, 8000.00),
(4, 'David', 3, 5500.00);

-- Insert Data into Departments
INSERT INTO departments (dept_id, dept_name) VALUES
(1, 'HR'),
(2, 'IT'),
(3, 'Finance');

-- Insert Data into Projects
INSERT INTO projects (project_id, project_name, emp_id) VALUES
(1, 'Project A', 1),
(2, 'Project B', 2),
(3, 'Project C', 1),
(4, 'Project D', 3);
```

### Inner Join
```sh
SELECT employees.emp_name, departments.dept_name
FROM employees
INNER JOIN departments ON employees.dept_id = departments.dept_id;
```

### Left Join
```sh
SELECT employees.emp_name, projects.project_name
FROM employees
LEFT JOIN projects ON employees.emp_id = projects.emp_id;
```

### Right Join
```sh
SELECT employees.emp_name, projects.project_name
FROM employees
RIGHT JOIN projects ON employees.emp_id = projects.emp_id;
```

### Full Outer Join

```sh
SELECT employees.emp_name, projects.project_name
FROM employees
LEFT JOIN projects ON employees.emp_id = projects.emp_id
UNION
SELECT employees.emp_name, projects.project_name
FROM employees
RIGHT JOIN projects ON employees.emp_id = projects.emp_id;
```

### Self Join

```sh
SELECT e1.emp_name AS Employee1, e2.emp_name AS Employee2, e1.dept_id
FROM employees e1
INNER JOIN employees e2 ON e1.dept_id = e2.dept_id AND e1.emp_id != e2.emp_id;
```

### Cross Joins

```sh
SELECT employees.emp_name, projects.project_name
FROM employees
CROSS JOIN projects;
```

### Complex Queries

```sh
SELECT e.emp_name, e.salary, p.project_name
FROM employees e
INNER JOIN projects p ON e.emp_id = p.emp_id
WHERE e.salary > 6000;
```

```sh
SELECT emp_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

```sh
SELECT d.dept_name, SUM(e.salary) AS total_salary
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id
GROUP BY d.dept_name;
```

```sh
DROP DATABASE testdb;
```

## 12. Advanced Commands for JOINS

```sh
-- Insert More Data into Employees
INSERT INTO employees (emp_id, emp_name, dept_id, salary) VALUES
(5, 'Eve', 2, 8500.00),
(6, 'Frank', 3, 6200.00),
(7, 'Grace', 1, 9000.00),
(8, 'Hannah', 4, 7500.00);

-- Insert More Data into Departments
INSERT INTO departments (dept_id, dept_name) VALUES
(4, 'Marketing');

-- Insert More Data into Projects
INSERT INTO projects (project_id, project_name, emp_id) VALUES
(5, 'Project E', 5),
(6, 'Project F', 6),
(7, 'Project G', 7),
(8, 'Project H', 8);
```

### Sort Top Employees
```sh
SELECT emp_name, salary
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

### Rank in departments
```sh
SELECT emp_name, dept_name, salary,
       RANK() OVER (PARTITION BY d.dept_name ORDER BY e.salary DESC) AS rank_in_department
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;
```

### Employees working on multiple projects
```sh
SELECT emp_name, COUNT(p.project_id) AS project_count
FROM employees e
JOIN projects p ON e.emp_id = p.emp_id
GROUP BY e.emp_id, e.emp_name
HAVING project_count > 1;
```

### Average Salary

```sh
SELECT d.dept_name, AVG(e.salary) AS avg_salary
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
GROUP BY d.dept_name
HAVING avg_salary > 7000;
```

### Employees not working on any projects

```sh
SELECT e.emp_name
FROM employees e
LEFT JOIN projects p ON e.emp_id = p.emp_id
WHERE p.project_id IS NULL;
```

### Employees working on same department as Alice

```sh
SELECT e2.emp_name
FROM employees e1
JOIN employees e2 ON e1.dept_id = e2.dept_id
WHERE e1.emp_name = 'Alice' AND e1.emp_id != e2.emp_id;
```

### Correlated Subquery: Find Employees Earning More Than the Average Salary in Their Department

```sh
SELECT e.emp_name, e.salary
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.dept_id = e.dept_id
);
```

### Common Table Expressions (CTE) to List Employees and Their Project Count

```sh
WITH EmployeeProjects AS (
    SELECT emp_id, COUNT(project_id) AS project_count
    FROM projects
    GROUP BY emp_id
)
SELECT e.emp_name, ep.project_count
FROM employees e
LEFT JOIN EmployeeProjects ep ON e.emp_id = ep.emp_id;
```

### Subquery with Joins: Find Departments Where All Employees Earn More Than 6000

```sh
SELECT dept_name
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.dept_id = d.dept_id
    AND e.salary <= 6000
);
```

### Advanced JOIN and Aggregate: Find the Project with the Highest Average Employee Salary

```sh
SELECT p.project_name, AVG(e.salary) AS avg_salary
FROM projects p
JOIN employees e ON p.emp_id = e.emp_id
GROUP BY p.project_id
ORDER BY avg_salary DESC
LIMIT 1;
```

### Join Across Multiple Tables: List Employees, Their Departments, and Projects

```sh
SELECT e.emp_name, d.dept_name, p.project_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
LEFT JOIN projects p ON e.emp_id = p.emp_id;
```

### Nested Queries with Joins: Find the Department with the Highest Total Salary

```sh
SELECT dept_name, total_salary
FROM (
    SELECT d.dept_name, SUM(e.salary) AS total_salary
    FROM employees e
    JOIN departments d ON e.dept_id = d.dept_id
    GROUP BY d.dept_name
) AS department_salaries
ORDER BY total_salary DESC
LIMIT 1;
```

### Cleanup function

```sh
-- Drop the Tables
DROP TABLE projects;
DROP TABLE employees;
DROP TABLE departments;

-- Drop the Database
DROP DATABASE testdb;
```

## 13. More Advanced Queries for Joins

### Recursive Common Table Expression (CTE)
```sh
-- Add Manager ID to Employees
ALTER TABLE employees ADD COLUMN manager_id INT DEFAULT NULL;

-- Update some sample data
UPDATE employees SET manager_id = 1 WHERE emp_id IN (2, 3);  -- Bob and Charlie report to Alice
UPDATE employees SET manager_id = 2 WHERE emp_id = 5;        -- Eve reports to Bob
UPDATE employees SET manager_id = 3 WHERE emp_id = 4;        -- David reports to Charlie

-- Recursive Query: Find the chain of command for each employee
WITH RECURSIVE EmployeeHierarchy AS (
    SELECT emp_id, emp_name, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL
    UNION ALL
    SELECT e.emp_id, e.emp_name, e.manager_id, eh.level + 1
    FROM employees e
    JOIN EmployeeHierarchy eh ON e.manager_id = eh.emp_id
)
SELECT * FROM EmployeeHierarchy;
```

### JSON Data Handling

```sh
-- Add a JSON column to the employees table
ALTER TABLE employees ADD COLUMN details JSON;

-- Update sample data to include JSON
UPDATE employees 
SET details = JSON_OBJECT('age', 30, 'hobbies', JSON_ARRAY('reading', 'sports'))
WHERE emp_id = 1;

-- Query to extract specific data from JSON
SELECT emp_name, JSON_UNQUOTE(JSON_EXTRACT(details, '$.age')) AS age
FROM employees
WHERE JSON_CONTAINS(details, '"reading"', '$.hobbies');
```

### Window Functions: Running Total
```sh
-- Calculate Running Total of Salaries Across All Employees
SELECT emp_name, salary,
       SUM(salary) OVER (ORDER BY emp_id) AS running_total
FROM employees;
```


### Advanced Grouping: GROUPING SETS, CUBE, and ROLLUP
```sh
-- Use ROLLUP to get the total salary per department and a grand total
SELECT d.dept_name, SUM(e.salary) AS total_salary
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
GROUP BY d.dept_name WITH ROLLUP;

-- Use CUBE to get total salary by both department and manager
SELECT d.dept_name, e.manager_id, SUM(e.salary) AS total_salary
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
GROUP BY CUBE(d.dept_name, e.manager_id);
```

### Lateral Join (Correlated Subquery in FROM)

```sh
-- For each employee, find the highest salary in their department
SELECT e.emp_name, e.salary, d.dept_name, max_salaries.max_salary
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
JOIN LATERAL (
    SELECT MAX(salary) AS max_salary
    FROM employees
    WHERE dept_id = e.dept_id
) AS max_salaries ON TRUE;
```

### Dynamic Pivoting

```sh
-- Example of dynamic pivoting to display total salaries by department and project
SELECT 
    d.dept_name,
    SUM(CASE WHEN p.project_name = 'Project A' THEN e.salary ELSE 0 END) AS ProjectA,
    SUM(CASE WHEN p.project_name = 'Project B' THEN e.salary ELSE 0 END) AS ProjectB,
    SUM(CASE WHEN p.project_name = 'Project C' THEN e.salary ELSE 0 END) AS ProjectC
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
LEFT JOIN projects p ON e.emp_id = p.emp_id
GROUP BY d.dept_name;
```

### Advanced Filtering: Filtering Based on Rank

```sh
-- Find the second-highest paid employee in each department
WITH RankedEmployees AS (
    SELECT emp_name, dept_id, salary,
           ROW_NUMBER() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS salary_rank
    FROM employees
)
SELECT emp_name, dept_id, salary
FROM RankedEmployees
WHERE salary_rank = 2;
```

### Advanced Subquery with EXISTS

```sh
-- Find departments where all employees are assigned to at least one project
SELECT dept_name
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    JOIN projects p ON e.emp_id = p.emp_id
    WHERE e.dept_id = d.dept_id
);
```

### Advanced Aggregate Functions: Percentile Calculations

```sh
-- Calculate the 75th percentile salary for employees
SELECT emp_name, salary,
       PERCENT_RANK() OVER (ORDER BY salary DESC) AS percentile_rank
FROM employees
WHERE PERCENT_RANK() OVER (ORDER BY salary DESC) >= 0.75;
```

### Common Table Expressions (CTEs) with Recursion for Date Ranges
```sh
-- Generate a range of dates using recursive CTE
WITH RECURSIVE DateRange AS (
    SELECT CURDATE() AS date
    UNION ALL
    SELECT date + INTERVAL 1 DAY
    FROM DateRange
    WHERE date + INTERVAL 1 DAY <= CURDATE() + INTERVAL 7 DAY
)
SELECT *
FROM DateRange;
```

### Full Text Search

```sh
-- Add a FULLTEXT index to a column for full-text search
ALTER TABLE employees ADD FULLTEXT(emp_name);

-- Use MATCH AGAINST for full-text search
SELECT emp_name
FROM employees
WHERE MATCH(emp_name) AGAINST ('Alice Bob' IN NATURAL LANGUAGE MODE);
```

### Multi-Table Update

```sh
-- Update employee salaries by department
UPDATE employees e
JOIN departments d ON e.dept_id = d.dept_id
SET e.salary = e.salary * 1.10
WHERE d.dept_name = 'IT';
```

### Stored Procedures with Dynamic SQL

```sh
DELIMITER //

CREATE PROCEDURE GetEmployeesBySalaryRange(IN min_salary DECIMAL(10,2), IN max_salary DECIMAL(10,2))
BEGIN
    SET @sql = CONCAT('SELECT emp_name, salary FROM employees WHERE salary BETWEEN ', min_salary, ' AND ', max_salary);
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //

DELIMITER ;

-- Call the procedure
CALL GetEmployeesBySalaryRange(6000, 9000);
```

### Conditional Aggregates with CASE

```sh
-- Find the total number of employees and those earning more than 7000 by department
SELECT d.dept_name,
       COUNT(*) AS total_employees,
       SUM(CASE WHEN e.salary > 7000 THEN 1 ELSE 0 END) AS high_earners
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
GROUP BY d.dept_name;
```

### Query Optimization: Using Indexes

```sh
-- Create index on the department ID in employees table for faster joins
CREATE INDEX idx_dept_id ON employees(dept_id);

-- Analyze the performance of a query using EXPLAIN
EXPLAIN SELECT e.emp_name, d.dept_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;
```