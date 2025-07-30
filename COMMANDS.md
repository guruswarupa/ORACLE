
# Oracle SQL Quick Reference (Extended)

## Database & Tables

- **Check Database Name**
  ```sql
  SELECT * FROM global_name;
  ```
  - Linux: `XEPDB1`
  - Windows: `XE`

- **List All Tables**
  ```sql
  SELECT * FROM tab;
  ```

- **Check Current User**
  ```sql  
  SHOW USER;
  ```

- **List All Tables (Current User)**
  ```sql
  SELECT table_name FROM user_tables;
  ```

- **List All Tables (All Users)**
  ```sql
  SELECT owner, table_name FROM all_tables;
  ```

- **Show All Users**
  ```sql
  SELECT username FROM all_users;
  ```

---

## Data Definition Language (DDL)

### Create a New Table
```sql
CREATE TABLE CUSTOMERS (
  id INT,
  name VARCHAR(30),
  age INT,
  address VARCHAR(80),
  salary DECIMAL(10,2)
);
```

### Check Table Structure
```sql
DESC CUSTOMERS;
```

### Alter Table

- **Modify Column**
  ```sql
  ALTER TABLE CUSTOMERS MODIFY salary DECIMAL(18,2) NOT NULL;
  ```

- **Add Column**
  ```sql
  ALTER TABLE CUSTOMERS ADD mobile INT NOT NULL;
  ```

### Drop Table
```sql
DROP TABLE CUSTOMERS;
```

### Create Table with Primary Key
```sql
CREATE TABLE CUSTOMERS (
  id INT NOT NULL,
  name VARCHAR2(20) NOT NULL,
  age INT NOT NULL,
  address VARCHAR2(25),
  salary DECIMAL(18,2),
  PRIMARY KEY (id)
);
```

### Add Constraint
```sql
ALTER TABLE CUSTOMERS ADD CONSTRAINT customCheck CHECK (age >= 18);
```

### Drop Constraint
```sql
ALTER TABLE CUSTOMERS DROP CONSTRAINT customCheck;
```

### Rename Table
```sql
RENAME CUSTOMERS TO CLIENTS;
```

### Drop Column
```sql
ALTER TABLE CUSTOMERS DROP COLUMN mobile;
```

### Rename Column
```sql
ALTER TABLE CUSTOMERS RENAME COLUMN name TO full_name;
```

### Truncate Table
```sql
TRUNCATE TABLE CUSTOMERS;
```

### Add Primary Key
```sql
ALTER TABLE CUSTOMERS ADD CONSTRAINT pk_id PRIMARY KEY (id);
```

### Add Foreign Key
```sql
ALTER TABLE ORDERS ADD CONSTRAINT fk_customer FOREIGN KEY (customer_id)
REFERENCES CUSTOMERS(id);
```

---

## Data Manipulation Language (DML)

### Select Data from Table
```sql
SELECT * FROM CUSTOMERS;
```

### Insert Data into Table
```sql
INSERT INTO CUSTOMERS(id, name, age, address, salary) 
VALUES (1, 'Tarun', 20, 'Bengaluru 560037', 120000);
```

### Update Data in Table
```sql
UPDATE CUSTOMERS SET name = 'Tarun' WHERE id = 1;
```

### Delete Data from Table
```sql
DELETE FROM CUSTOMERS WHERE id = 1;
```

### Insert Multiple Rows
```sql
INSERT ALL
  INTO CUSTOMERS(id, name, age, address, salary) VALUES (2, 'Guru', 25, 'Chennai', 80000)
  INTO CUSTOMERS(id, name, age, address, salary) VALUES (3, 'Swarupa', 23, 'Mumbai', 70000)
SELECT * FROM dual;
```

### Select Specific Columns
```sql
SELECT name, salary FROM CUSTOMERS;
```

### Conditional Select
```sql
SELECT * FROM CUSTOMERS WHERE age > 25 AND salary >= 50000;
```

### Order By
```sql
SELECT * FROM CUSTOMERS ORDER BY salary DESC;
```

### Like Operator
```sql
SELECT * FROM CUSTOMERS WHERE name LIKE 'G%';
```

### Between Operator
```sql
SELECT * FROM CUSTOMERS WHERE age BETWEEN 20 AND 30;
```

### In Operator
```sql
SELECT * FROM CUSTOMERS WHERE name IN ('Tarun', 'Guru');
```

---

## Aggregate Functions

### Count Rows
```sql
SELECT COUNT(*) FROM CUSTOMERS;
```

### Sum, Average, Min, Max
```sql
SELECT SUM(salary), AVG(salary), MIN(salary), MAX(salary) FROM CUSTOMERS;
```

### Group By
```sql
SELECT age, COUNT(*) FROM CUSTOMERS GROUP BY age;
```

### Having Clause
```sql
SELECT age, COUNT(*) FROM CUSTOMERS GROUP BY age HAVING COUNT(*) > 1;
```

---

## Joins

### Inner Join
```sql
SELECT C.name, O.order_id 
FROM CUSTOMERS C
JOIN ORDERS O ON C.id = O.customer_id;
```

### Left Join
```sql
SELECT C.name, O.order_id 
FROM CUSTOMERS C
LEFT JOIN ORDERS O ON C.id = O.customer_id;
```

### Right Join
```sql
SELECT C.name, O.order_id 
FROM CUSTOMERS C
RIGHT JOIN ORDERS O ON C.id = O.customer_id;
```

---

## Subqueries

### Subquery in WHERE Clause
```sql
SELECT * FROM CUSTOMERS 
WHERE salary > (SELECT AVG(salary) FROM CUSTOMERS);
```

---

## Views

### Create View
```sql
CREATE VIEW high_earners AS 
SELECT * FROM CUSTOMERS WHERE salary > 100000;
```

### Select from View
```sql
SELECT * FROM high_earners;
```

### Drop View
```sql
DROP VIEW high_earners;
```

---

## Users & Privileges

### Create User
```sql
CREATE USER testuser IDENTIFIED BY password123;
```

### Grant Privileges
```sql
GRANT CONNECT, RESOURCE TO testuser;
```

### Grant All Privileges
```sql
GRANT ALL PRIVILEGES TO testuser;
```

### Revoke Privileges
```sql
REVOKE CONNECT FROM testuser;
```

---

## Additional SQL Features

### Alias
```sql
SELECT name AS customer_name, salary AS monthly_salary FROM CUSTOMERS;
```

### DISTINCT
```sql
SELECT DISTINCT age FROM CUSTOMERS;
```

### CASE Statement
```sql
SELECT name,
       CASE 
         WHEN salary > 100000 THEN 'High'
         WHEN salary BETWEEN 50000 AND 100000 THEN 'Medium'
         ELSE 'Low'
       END AS salary_band
FROM CUSTOMERS;
```

### IS NULL / IS NOT NULL
```sql
SELECT * FROM CUSTOMERS WHERE address IS NULL;
```
```sql
SELECT * FROM CUSTOMERS WHERE address IS NOT NULL;
```

### EXISTS
```sql
SELECT * FROM CUSTOMERS C
WHERE EXISTS (
  SELECT 1 FROM ORDERS O WHERE O.customer_id = C.id
);
```

### UNION / UNION ALL
```sql
SELECT name FROM CUSTOMERS
UNION
SELECT name FROM EMPLOYEES;
```
```sql
SELECT name FROM CUSTOMERS
UNION ALL
SELECT name FROM EMPLOYEES;
```

### ROWNUM
```sql
SELECT * FROM CUSTOMERS WHERE ROWNUM <= 5;
```

### FETCH FIRST N ROWS (Oracle 12c+)
```sql
SELECT * FROM CUSTOMERS FETCH FIRST 5 ROWS ONLY;
```

---

## Transactions

### Commit
```sql
COMMIT;
```

### Rollback
```sql
ROLLBACK;
```

---

## Indexes

### Create Index
```sql
CREATE INDEX idx_name ON CUSTOMERS(name);
```

### Drop Index
```sql
DROP INDEX idx_name;
```

---

## Sequences

### Create Sequence
```sql
CREATE SEQUENCE cust_seq START WITH 1 INCREMENT BY 1;
```

### Use Sequence in Insert
```sql
INSERT INTO CUSTOMERS(id, name, age, address, salary) 
VALUES (cust_seq.NEXTVAL, 'Aman', 26, 'Delhi', 90000);
```