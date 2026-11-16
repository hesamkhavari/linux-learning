# SQL Basics with MySQL

## 1. Database Concepts
A database is an organized collection of data.

A relational database stores data in tables.

- **Database** : Contains tables.

- **Table** : Contains rows and columns.

- **Row** : One record.

- **Column** : One field.

- **Primary Key** : Uniquely identifies each row.

- **Foreign Key** : Connects related tables.

## 2. Install and Start MySQL
On Ubuntu or Debian :
```bash
sudo apt update
sudo apt install mysql-server
```
Start the service :
```bash
sudo systemctl start mysql
```
Check its status :
```bash
sudo systemctl status mysql
```
Enable MySQL at startup :
```bash
sudo systemctl enable mysql
```
Stop or restart :
```bash
sudo systemctl stop mysql
sudo systemctl restart mysql
```

## 3. Connect to MySQL
On Ubuntu, root may use socket authentication :
```bash
sudo mysql
```
Connect with a password :
```bash
mysql -u root -p
```

- `-u` : Username

- `-p` : Ask for a password

Exit MySQL:
```bash
EXIT;
```
or:
```bash
\q
```

**Note**: `Ctrl+C` cancels the current input or query. It is not the normal way to exit MySQL.

## 4. Database Management
Show all databases :
```bash
SHOW DATABASES;
```
Create a database :
```bash
CREATE DATABASE infra_db;
```
Select a database :
```bash
USE infra_db;
```
Delete a database :
```bash
DROP DATABASE infra_db;
```

**Warning** : `DROP DATABASE` permanently removes the database and its tables.

## 5. Table Management
Show tables :
```bash
SHOW TABLES;
```
Create a table :
```bash
CREATE TABLE forums (
    id INT PRIMARY KEY AUTO_INCREMENT,
    forum VARCHAR(50),
    video INT
);
```
Create another table :
```bash
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL,
    record INT,
    activity VARCHAR(20)
);
```
Show table structure :
```bash
DESCRIBE forums;
```
or :
```bash
SHOW COLUMNS FROM forums;
```
Delete a table :
```bash
DROP TABLE forums;
```
## 6. Data Types

Data Type|Purpose
|---|---|
INT|Integer numbers
BIGINT|Large integer numbers
CHAR(n)|Fixed-length text
VARCHAR(n)|Variable-length text
TEXT|Long text
DATE|Date
DATETIME|Date and time
DECIMAL(p,s)|Exact decimal numbers

> **Note** : `VARCHAR` is usually a good choice for names and other text with different lengths.

## 7. Insert Data (INSERT)
Insert one record :
```bash
INSERT INTO forums (forum, video)
VALUES ('network', 500);
```
Insert multiple records :
```bash
INSERT INTO forums (forum, video)
VALUES
('linux', 600),
('icdl', 700),
('graphic', 800);
```
View all records :
```bash
SELECT * FROM forums;
```

> **Important** : `AUTO_INCREMENT` creates the ID automatically.

Always specify column names when inserting data. This makes queries easier to understand and safer when table structures change.

## 8. Read Data (SELECT)
Select all columns :
```bash
SELECT * FROM forums;
```
Select specific columns :
```bash
SELECT id, forum FROM forums;
```
Select unique values :
```bash
SELECT DISTINCT activity FROM users;
```
Count all records :
```bash
SELECT COUNT(*) FROM users;
```
Limit the number of results :
```bash
SELECT * FROM forums LIMIT 3;
```

## 9. Filter Data (WHERE)
Find records with a specific value :
```bash
SELECT * FROM forums
WHERE forum = 'linux';
```
Not equal :
```bash
SELECT * FROM forums
WHERE forum != 'linux';
```
### LIKE Operator
`LIKE` searches for text patterns.

Pattern|Meaning
|---|---|
`'l%'`|Starts with l
`'%i%'`|Contains i
`'%c'`|Ends with c
`'_i%'`|Second character is i

Examples :
```bash
SELECT * FROM forums WHERE forum LIKE 'l%';

SELECT * FROM forums WHERE forum LIKE '%i%';

SELECT * FROM forums WHERE forum LIKE '%c';
```
### AND / OR / NOT
```bash
SELECT * FROM forums
WHERE forum LIKE '%i%' AND id < 3;
```
```bash
SELECT * FROM forums
WHERE forum = 'linux' OR forum = 'network';
```
```bash
SELECT * FROM forums
WHERE NOT forum = 'linux';
```
Other useful operators :
```bash
WHERE id BETWEEN 1 AND 3;

WHERE forum IN ('linux', 'network');

WHERE username IS NULL;

WHERE username IS NOT NULL;
```

## 10. Sort Data (ORDER BY)
Ascending order (default) :
```bash
SELECT * FROM forums
ORDER BY id ASC;
```
Descending order :
```bash
SELECT * FROM forums
ORDER BY id DESC;
```
Sort by multiple columns :
```bash
SELECT * FROM forums
ORDER BY forum ASC, id DESC;
```

## 11. Group Data (GROUP BY)
`GROUP BY` groups rows with the same value.

Count users in each activity group :
```bash
SELECT activity, COUNT(*)
FROM users
GROUP BY activity;
```
Calculate the average record :
```bash
SELECT activity, AVG(record)
FROM users
GROUP BY activity;
```
Other aggregate functions :

Function|Purpose
|---|---|
`COUNT()`|Count records
`SUM()`|Calculate total
`AVG()`|Calculate average
`MIN()`|Find minimum
`MAX()`|Find maximum

Filter grouped results with `HAVING` :
```bash
SELECT activity, COUNT(*)
FROM users
GROUP BY activity
HAVING COUNT(*) > 1;
```
Difference :
- `WHERE` filters rows before grouping.

- `HAVING` filters groups after grouping.

## 12. Update Data (UPDATE)
Change a value :
```bash
UPDATE users
SET username = 'HESAM'
WHERE username = 'hesam';
```
