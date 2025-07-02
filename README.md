# Library_Management_System
## Project Overview:
Project Title: Library Management System                                                                                                                              
Database: library_db

This project showcases a comprehensive Library Management System implemented using SQL. It focuses on database design and management,
featuring the creation and maintenance of tables, execution of CRUD (Create, Read, Update, Delete) operations, and advanced SQL querying. 
The system demonstrates proficiency in structuring a relational database to efficiently manage library operations, highlighting skills in 
data manipulation and complex query execution

   ![library managment system](https://github.com/user-attachments/assets/35bdce61-b45c-4f90-9635-43bdb377b9c3)


- **Set up the Library Management System Database**:  
  - Create a database for the library.  
  - Build tables for branches, employees, members, books, issued status, and return status.  
  - Populate these tables with relevant data.  

- **CRUD Operations**:  
  - **Create**: Add new records to tables (e.g., new books or members).  
  - **Read**: Retrieve data from tables (e.g., list all books).  
  - **Update**: Modify existing data (e.g., update member details).  
  - **Delete**: Remove records from tables (e.g., delete inactive members).  

- **CTAS (Create Table As Select)**:  
  - Use CTAS to create new tables based on the results of specific queries (e.g., a table of overdue books).  

- **Advanced SQL Queries**:  
  - Write complex queries to analyze data (e.g., find most borrowed books or active members).  
  - Retrieve specific information using joins, filters, and aggregations.
 
 ## Project Structure:-
 1. Database Setup
  ![library_erd](https://github.com/user-attachments/assets/b167145d-1f32-4374-8445-cef9da386a22)

- **Database Creation:** Created a database named library_db.
- **Table Creation:** Created tables for branches, employees, members, books, issued status, and return status. 
Each table includes relevant columns and relationships.

```sql
CREATE DATABASE library_db;

DROP TABLE IF EXISTS branch;
CREATE TABLE branch
(
            branch_id VARCHAR(10) PRIMARY KEY,
            manager_id VARCHAR(10),
            branch_address VARCHAR(30),
            contact_no VARCHAR(15)
);


-- Create table "Employee"
DROP TABLE IF EXISTS employees;
CREATE TABLE employees
(
            emp_id VARCHAR(10) PRIMARY KEY,
            emp_name VARCHAR(30),
            position VARCHAR(30),
            salary DECIMAL(10,2),
            branch_id VARCHAR(10),
            FOREIGN KEY (branch_id) REFERENCES  branch(branch_id)
);


-- Create table "Members"
DROP TABLE IF EXISTS members;
CREATE TABLE members
(
            member_id VARCHAR(10) PRIMARY KEY,
            member_name VARCHAR(30),
            member_address VARCHAR(30),
            reg_date DATE
);



-- Create table "Books"
DROP TABLE IF EXISTS books;
CREATE TABLE books
(
            isbn VARCHAR(50) PRIMARY KEY,
            book_title VARCHAR(80),
            category VARCHAR(30),
            rental_price DECIMAL(10,2),
            status VARCHAR(10),
            author VARCHAR(30),
            publisher VARCHAR(30)
);



-- Create table "IssueStatus"
DROP TABLE IF EXISTS issued_status;
CREATE TABLE issued_status
(
            issued_id VARCHAR(10) PRIMARY KEY,
            issued_member_id VARCHAR(30),
            issued_book_name VARCHAR(80),
            issued_date DATE,
            issued_book_isbn VARCHAR(50),
            issued_emp_id VARCHAR(10),
            FOREIGN KEY (issued_member_id) REFERENCES members(member_id),
            FOREIGN KEY (issued_emp_id) REFERENCES employees(emp_id),
            FOREIGN KEY (issued_book_isbn) REFERENCES books(isbn) 
);



-- Create table "ReturnStatus"
DROP TABLE IF EXISTS return_status;
CREATE TABLE return_status
(
            return_id VARCHAR(10) PRIMARY KEY,
            issued_id VARCHAR(30),
            return_book_name VARCHAR(80),
            return_date DATE,
            return_book_isbn VARCHAR(50),
            FOREIGN KEY (return_book_isbn) REFERENCES books(isbn)
);
```
## 2. CRUD Operations ##
- **Create:** Inserted sample records into the books table.
- **Read:** Retrieved and displayed data from various tables.
- **Update:** Updated records in the employees table.
- **Delete:** Removed records from the members table as needed.
  
**Task 1. Create a New Book Record** -- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee','J.B. Lippincott & Co.')"
```sql
INSERT INTO books(isbn, book_title, category, rental_price, status, author, publisher)
VALUES('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
SELECT * FROM books;
```
**Task 2**: Update an Existing Member's Address
```sql 
UPDATE members
SET member_address = '125 Oak St'
WHERE member_id = 'C103';
```
**Task 3**: Delete a Record from the Issued Status Table -- Objective: Delete the record with issued_id = 'IS121' from the issued_status table.
```sql
DELETE FROM issued_status
WHERE   issued_id =   'IS121';
```
**Task 4**: Retrieve All Books Issued by a Specific Employee -- Objective: Select all books issued by the employee with emp_id = 'E101'.
```sql
SELECT * FROM issued_status
WHERE issued_emp_id = 'E101'
```
**Task 5**: List Members Who Have Issued More Than One Book -- Objective: Use GROUP BY to find members who have issued more than one book.
```sql
SELECT
    issued_emp_id,
    COUNT(*)
FROM issued_status
GROUP BY 1
HAVING COUNT(*) > 1
```
## 3. CTAS (Create Table As Select)
- **Task 6**: Create Summary Tables: Used CTAS to generate new tables based on query results - each book and total book_issued_cnt
```sql
CREATE TABLE book_issued_cnt AS
SELECT b.isbn, b.book_title, COUNT(ist.issued_id) AS issue_count
FROM issued_status as ist
JOIN books as b
ON ist.issued_book_isbn = b.isbn
GROUP BY b.isbn, b.book_title;
```
## 4. Data Analysis & Findings
The following SQL queries were used to address specific questions:

**Task 7**. Retrieve All Books in a Specific Category:
```sql
SELECT * FROM books
WHERE category = 'Classic';
```
**Task 8**: Find Total Rental Income by Category:
```sql
SELECT 
    b.category,
    SUM(b.rental_price),
    COUNT(*)
FROM 
issued_status as ist
JOIN
books as b
ON b.isbn = ist.issued_book_isbn
GROUP BY 1
```
**Task 9**: List Members Who Registered in the Last 180 Days:
```sql
SELECT * FROM members
WHERE reg_date >= CURRENT_DATE - INTERVAL '180 days';
```
**Task 10**: List Employees with Their Branch Manager's Name and their branch details:
```sql
SELECT 
    e1.emp_id,
    e1.emp_name,
    e1.position,
    e1.salary,
    b.*,
    e2.emp_name as manager
FROM employees as e1
JOIN 
branch as b
ON e1.branch_id = b.branch_id    
JOIN
employees as e2
ON e2.emp_id = b.manager_id
```
**Task 11**. Create a Table of Books with Rental Price Above a Certain Threshold:
```sql
CREATE TABLE expensive_books AS
SELECT * FROM books
WHERE rental_price > 7.00;
```
**Task 12**: Retrieve the List of Books Not Yet Returned
```sql
SELECT * FROM issued_status as ist
LEFT JOIN
return_status as rs
ON rs.issued_id = ist.issued_id
WHERE rs.return_id IS NULL;
```
## Advanced SQL Operations
**Task 13**: Identify Members with Overdue Books
Write a query to identify members who have overdue books (assume a 30-day return period). Display the member's_id, member's name, book title, issue date, and days overdue.
```sql
SELECT 
    ist.issued_member_id,
    m.member_name,
    bk.book_title,
    ist.issued_date,
    -- rs.return_date,
    CURRENT_DATE - ist.issued_date as over_dues_days
FROM issued_status as ist
JOIN 
members as m
    ON m.member_id = ist.issued_member_id
JOIN 
books as bk
ON bk.isbn = ist.issued_book_isbn
LEFT JOIN 
return_status as rs
ON rs.issued_id = ist.issued_id
WHERE 
    rs.return_date IS NULL
    AND
    (CURRENT_DATE - ist.issued_date) > 30
ORDER BY 1
```
**Task 14**: Update Book Status on Return                                                                                                                           Write a query to update the status of books in the books table to "Yes" when they are returned (based on entries in the return_status table).

