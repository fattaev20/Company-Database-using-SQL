# Company-Database-using-SQL

In this project we are creating a Database for storing information of the Company according to the specifications

### Table of Contents:
 - [00. Project Overview](#project-overview)
 - [01. Creating ER diagram from Specifications](#creating-er-diagram-from-specifications)
 - [02. Converting ER diagram to database schema](#converting-er-diagram-to-database-schema)
 - [03. SQL code of the Database schema](#sql-code-of-the-database-schema)
 - [04. Analyzing results and Next steps](#analyzing-results-and-next-steps)
 - [05. Credits](#credits)
---

### Project Overview
The Company gives us a specifications list to create a database for it. The specifications include followings:
> The company is organized into branches. Each branch has a unique number, a name, and a particular employee who manages it. 
>
> The company makes it’s money by selling to clients. Each client has a name and a unique number to identify it. 
>
> The foundation of the company is it’s employees. Each employee has a name, birthday, sex, salary and a unique number.
> 
> An employee can work for one branch at a time, and each branch will be managed by one of the employees that work there. We’ll also want to keep track of when the current manager started as manager.
> 
> An employee can act as a supervisor for other employees at the branch, an employee may also act as the supervisor for employees at other branches. An employee can have at most one supervisor.
> 
> A branch may handle a number of clients, with each client having a name and a unique number to identify it. A single client may only be handled by one branch at a time.
> 
> Employees can work with clients controlled by their branch to sell them stuff. If nescessary multiple employees can work with the same client. We’ll want to keep track of how many dollars worth of stuff each employee sells to each client they work with. 
>
> Many branches will need to work with suppliers to buy inventory. For each supplier we’ll keep track of their name and the type of product they’re selling the branch. A single supplier may supply products to multiple branches.

Our task is to create a database for tthe Company above to fulfill their needs.

---

### Creating ER diagram from Specifications
According to the specifications provided we created the ER diagram below that shows how tables in our database organized and interact with each other:

![image](https://github.com/user-attachments/assets/6ce1a191-97b9-47ae-a499-7f0d698c92bd)

---

### Converting ER diagram to database schema
Using our ER diagram we created a schema for our future database:

![image](https://github.com/user-attachments/assets/eb6af334-b741-413f-82f6-338e52a4255e)

Above we can see 5 tables and their structure, where:
- _Red - Primary key_
- _Blue - Simple Column_
- _Green - Foreign key_

---

### SQL code of the Database schema
Using SQL we can create our schema at MySQL DBMS 

```SQL
CREATE TABLE employee (
  emp_id INT PRIMARY KEY,
  first_name VARCHAR(40),
  last_name VARCHAR(40),
  birth_day DATE,
  sex VARCHAR(1),
  salary INT,
  super_id INT,
  branch_id INT
);

CREATE TABLE branch (
  branch_id INT PRIMARY KEY,
  branch_name VARCHAR(40),
  mgr_id INT,
  mgr_start_date DATE,
  FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
);

ALTER TABLE employee
ADD FOREIGN KEY(branch_id)
REFERENCES branch(branch_id)
ON DELETE SET NULL;

ALTER TABLE employee
ADD FOREIGN KEY(super_id)
REFERENCES employee(emp_id)
ON DELETE SET NULL;

CREATE TABLE client (
  client_id INT PRIMARY KEY,
  client_name VARCHAR(40),
  branch_id INT,
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL
);

CREATE TABLE works_with (
  emp_id INT,
  client_id INT,
  total_sales INT,
  PRIMARY KEY(emp_id, client_id),
  FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
  FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
);

CREATE TABLE branch_supplier (
  branch_id INT,
  supplier_name VARCHAR(40),
  supply_type VARCHAR(40),
  PRIMARY KEY(branch_id, supplier_name),
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE
);

```
---

### Analyzing results and Next steps
Overall, we analyzed company specifications, designed ER diagram and turned it into a Database Schema which later was tured into a real Database using SQL.

Further steps to improve it may include:
01. Inserting information properly to the database
02. Adding new tables according to needs of the Company
03. Creating Triggers that could automate repetative processes in database

---

### Credits
_Special credits for Giraffe Academy for guidance and help through the process of creating database_


