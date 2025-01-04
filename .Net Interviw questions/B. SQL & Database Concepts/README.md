# .NET Interview Questions

This repository contains a comprehensive list of commonly asked .NET interview questions, categorized for ease of reference.

---


## B. SQL & Database Concepts

1. **Relationship Types**


In relational databases, relationship types define how data is related to one another across different tables. The primary types of relationships are:

- **One-to-One (1:1)**: In a one-to-one relationship, a single record in one table is associated with a single record in another table. This is often used when an entity has optional attributes that are better stored in a separate table.

  **Example**: A `User` table and a `Profile` table where each user has exactly one profile.

  ```sql
  CREATE TABLE Users (
      UserID INT PRIMARY KEY,
      UserName VARCHAR(50) NOT NULL
  );

  CREATE TABLE Profiles (
      ProfileID INT PRIMARY KEY,
      UserID INT UNIQUE,
      Bio TEXT,
      FOREIGN KEY (UserID) REFERENCES Users(UserID)
  );


#### One-to-Many (1:N)

In a one-to-many relationship, a single record in one table can be associated with multiple records in another table. This is the most common type of relationship.

**Example**: A `Customer` table and an `Order` table where a customer can have multiple orders.

```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(50) NOT NULL
);

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    OrderDate DATETIME,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

#### Many-to-Many (M:N)

In a many-to-many relationship, multiple records in one table can be associated with multiple records in another table. This type of relationship requires a junction table to manage the associations between the two tables.

**Example**: A `Student` table and a `Course` table where students can enroll in multiple courses, and each course can have multiple students.

```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(50) NOT NULL
);

CREATE TABLE Courses (
    CourseID INT PRIMARY KEY,
    CourseName VARCHAR(50) NOT NULL
);

CREATE TABLE Enrollments (
    StudentID INT,
    CourseID INT,
    PRIMARY KEY (StudentID, CourseID),
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);


2. **Creating Many-to-Many Relationships Between Tables**


To establish a many-to-many relationship in a relational database, you need to create a junction (or associative) table that connects the two tables involved in the relationship. This junction table will contain foreign keys referencing the primary keys of the two related tables.

**Example**: Consider a scenario where you have a `Student` table and a `Course` table. Each student can enroll in multiple courses, and each course can have multiple students. To implement this, you will need to create an `Enrollments` table as a junction table.

Here is how you can define these tables in SQL:

```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(50) NOT NULL
);

CREATE TABLE Courses (
    CourseID INT PRIMARY KEY,
    CourseName VARCHAR(50) NOT NULL
);

CREATE TABLE Enrollments (
    StudentID INT,
    CourseID INT,
    PRIMARY KEY (StudentID, CourseID),
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);

>>> 
- The **Students** table holds information about students.  
- The **Courses** table holds information about courses.  
- The **Enrollments** table establishes the many-to-many relationship by linking `StudentID` and `CourseID`.

3. **Difference Between Primary Key and Foreign Key**


| Feature               | Primary Key                                          | Foreign Key                                         |
|-----------------------|------------------------------------------------------|----------------------------------------------------|
| Definition            | A column or a set of columns that uniquely identifies each row in a table. | A column or a set of columns that establishes a link between data in two tables. |
| Uniqueness            | Must contain unique values for each row.            | Can contain duplicate values and can also be null. |
| Number of Keys        | Each table can have only one primary key.           | A table can have multiple foreign keys.            |
| Purpose               | Ensures the entity integrity of a table.            | Maintains referential integrity between two tables. |

**Example:**

```sql
-- Primary Key Example
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(50) NOT NULL
);

-- Foreign Key Example
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);


4. **SELECT Statement**

The `SELECT` statement is used to query data from a database. It allows you to specify which columns of data you want to retrieve from one or more tables.

**Basic Syntax:**

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;

**Example:**

```sql
SELECT CustomerName, OrderDate
FROM Orders
WHERE CustomerID = 1;

5. **Difference Between WHERE Clause and HAVING Clause**

| Feature               | WHERE Clause                                       | HAVING Clause                                    |
|-----------------------|----------------------------------------------------|-------------------------------------------------|
| Purpose               | Filters records before any groupings are made.    | Filters records after grouping has occurred.    |
| Usage                 | Can be used with `SELECT`, `UPDATE`, and `DELETE`. | Can only be used with `SELECT` statements that include `GROUP BY`. |
| Condition Type        | Can filter based on any column.                    | Typically used to filter aggregated data.       |

**Example:**

```sql
-- Using WHERE Clause
SELECT CustomerName
FROM Customers
WHERE Country = 'USA';

-- Using HAVING Clause
SELECT Country, COUNT(CustomerID) AS NumberOfCustomers
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 10;


6. **JOINs**

JOINs are used to combine rows from two or more tables based on a related column between them. Different types of JOINs determine how rows from the tables are matched.

| Type of JOIN          | Description                                          |
|-----------------------|-----------------------------------------------------|
| INNER JOIN            | Returns records that have matching values in both tables. |
| LEFT JOIN (or LEFT OUTER JOIN) | Returns all records from the left table and the matched records from the right table. If there is no match, NULL values are returned for columns from the right table. |
| RIGHT JOIN (or RIGHT OUTER JOIN) | Returns all records from the right table and the matched records from the left table. If there is no match, NULL values are returned for columns from the left table. |
| FULL JOIN (or FULL OUTER JOIN) | Returns all records when there is a match in either left or right table records. |

**Examples:**

```sql
-- INNER JOIN Example
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID;

-- LEFT JOIN Example
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID;

-- RIGHT JOIN Example
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
RIGHT JOIN Orders ON Customers.CustomerID = Orders.CustomerID;

-- FULL JOIN Example
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders ON Customers.CustomerID = Orders.CustomerID;


7. **Indexes: Clustered vs Non-Clustered**

Indexes are used to speed up the retrieval of rows from a database table. There are two main types of indexes: clustered and non-clustered.

| Feature               | Clustered Index                                   | Non-Clustered Index                                 |
|-----------------------|---------------------------------------------------|-----------------------------------------------------|
| Definition            | Determines the physical order of data in the table. There can only be one clustered index per table. | A separate structure that holds a pointer to the data, allowing multiple non-clustered indexes on a table. |
| Data Storage          | The actual data rows are stored in the order of the clustered index. | The data remains in its original order, with pointers stored in the non-clustered index. |
| Performance           | Faster for range queries, as data is stored sequentially. | Can be slower for range queries but can improve performance for specific lookups. |

**Examples:**

```sql
-- Creating a Clustered Index
CREATE CLUSTERED INDEX idx_CustomerID ON Customers(CustomerID);

-- Creating a Non-Clustered Index
CREATE NONCLUSTERED INDEX idx_CustomerName ON Customers(CustomerName);


8. **Normalization (1NF, 2NF, 3NF)**

Normalization is the process of organizing data in a database to reduce redundancy and improve data integrity. The most common normal forms are First Normal Form (1NF), Second Normal Form (2NF), and Third Normal Form (3NF).

| Normal Form          | Description                                          |
|----------------------|-----------------------------------------------------|
| First Normal Form (1NF) | Ensures that each column contains atomic (indivisible) values, and each record is unique. There are no repeating groups or arrays. |
| Second Normal Form (2NF) | Achieves 1NF and ensures that all non-key attributes are fully functionally dependent on the primary key, eliminating partial dependencies. |
| Third Normal Form (3NF) | Achieves 2NF and ensures that all the attributes are only dependent on the primary key, eliminating transitive dependencies. |

**Examples:**

```sql
-- Example of a table in 1NF
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(50) NOT NULL,
    Course VARCHAR(50) NOT NULL  -- Atomic values
);

-- Example of a table in 2NF
CREATE TABLE Courses (
    CourseID INT PRIMARY KEY,
    CourseName VARCHAR(50) NOT NULL
);

CREATE TABLE Enrollments (
    StudentID INT,
    CourseID INT,
    PRIMARY KEY (StudentID, CourseID),
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);

-- Example of a table in 3NF
CREATE TABLE Professors (
    ProfessorID INT PRIMARY KEY,
    ProfessorName VARCHAR(50) NOT NULL,
    Department VARCHAR(50) NOT NULL  -- No transitive dependency
);


9. **Cursors**

Cursors are database objects used to retrieve, manipulate, and navigate through a result set row by row. They are particularly useful when you need to process individual rows returned by a query.

| Feature               | Description                                          |
|-----------------------|-----------------------------------------------------|
| Purpose               | Allow row-by-row processing of query results.       |
| Types                 | Implicit and explicit cursors.                      |
| Performance           | Can be slower than set-based operations due to row-by-row processing. |

**Example of Using a Cursor:**

```sql
-- Declaring a Cursor
DECLARE @StudentName VARCHAR(50);
DECLARE student_cursor CURSOR FOR
SELECT StudentName FROM Students;

-- Opening the Cursor
OPEN student_cursor;

-- Fetching Rows
FETCH NEXT FROM student_cursor INTO @StudentName;

WHILE @@FETCH_STATUS = 0
BEGIN
    PRINT @StudentName;  -- Process the fetched row
    FETCH NEXT FROM student_cursor INTO @StudentName;
END;

-- Closing and Deallocating the Cursor
CLOSE student_cursor;
DEALLOCATE student_cursor;


10. **Stored Procedures vs User-Defined Functions**

Stored Procedures and User-Defined Functions (UDFs) are both types of routines that can be created to encapsulate SQL code for reuse, but they serve different purposes and have distinct characteristics.

| Feature               | Stored Procedures                                  | User-Defined Functions (UDFs)                     |
|-----------------------|---------------------------------------------------|---------------------------------------------------|
| Purpose               | Perform actions such as modifying data and executing complex logic. | Return a single value or a table and are used in SQL statements. |
| Execution             | Can include control-of-flow logic (e.g., IF statements, loops) and can modify the database state. | Cannot modify database state; primarily used for computations and returning values. |
| Return Type           | Does not return a value directly; output parameters can be used. | Must return a value (scalar) or a table (table-valued). |
| Calling Method        | Called using `EXEC` or `EXECUTE` statement.      | Can be called as part of a SQL statement, like a built-in function. |

**Example of a Stored Procedure:**

```sql
CREATE PROCEDURE GetStudentCount
AS
BEGIN
    SELECT COUNT(*) AS TotalStudents FROM Students;
END;

**Example of a User-Defined Function:**

```sql
CREATE FUNCTION GetStudentFullName (@StudentID INT)
RETURNS VARCHAR(100)
AS
BEGIN
    DECLARE @FullName VARCHAR(100);
    SELECT @FullName = StudentName + ' - ' + CAST(StudentID AS VARCHAR)
    FROM Students
    WHERE StudentID = @StudentID;
    RETURN @FullName;
END;


11. **SQL Injection and Its Solutions**

SQL Injection is a code injection technique that attackers use to exploit vulnerabilities in an application's software by inserting malicious SQL statements into a query. This can lead to unauthorized access to sensitive data, data manipulation, and even complete database compromise.

| Vulnerability          | Description                                          |
|-----------------------|-----------------------------------------------------|
| SQL Injection          | Occurs when user inputs are not properly sanitized, allowing attackers to modify SQL queries. |

**Example of a Vulnerable SQL Query:**

```sql
-- Vulnerable SQL Query
DECLARE @UserInput VARCHAR(50) = 'some_user_input';
EXEC('SELECT * FROM Users WHERE Username = ''' + @UserInput + '''');


**Solutions to Prevent SQL Injection:**

1. **Use Prepared Statements:** Prepared statements ensure that SQL code and data are separated, preventing attackers from injecting malicious SQL.

   ```sql
   -- Example of a Prepared Statement
   DECLARE @UserInput VARCHAR(50) = 'some_user_input';
   EXEC sp_executesql N'SELECT * FROM Users WHERE Username = @Username', N'@Username VARCHAR(50)', @Username = @UserInput;


**Use Parameterized Queries:** Parameterized queries automatically handle input validation and sanitization.

```sql
-- Example of a Parameterized Query
SELECT * FROM Users WHERE Username = @Username;

**Input Validation:** Always validate user inputs to ensure they conform to expected formats (e.g., length, type, format).

**Limit Database Permissions:** Use the principle of least privilege to restrict the database permissions of the application account.

**Regular Security Audits:** Conduct regular security audits and code reviews to identify and remediate vulnerabilities.
