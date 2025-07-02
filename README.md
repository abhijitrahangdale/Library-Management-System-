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
-** Create:** Inserted sample records into the books table.
- **Read:** Retrieved and displayed data from various tables.
- **Update:** Updated records in the employees table.
- **Delete:** Removed records from the members table as needed.
