# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
    The main goal of the project is to transform data into Pgadmin from another source(like CSV files) and after that, analyze data in the database.
    To make data organized and concise thus, we can get accurate output from the database.
    It is based on Real life project so this project helps to improve SQL skills, Data cleaning as well as QA skills.

    
## Process
### Step 1:: Tool Installation & Set Up
    Install PGAdmin for creating databases and running SQL queries.
    Install Visual Studio code to edit the files which download from GitHub.

### Step 2::Import CSV Files Into PGadmin
    Create a database of E-commerce in PGAdmin.
    Create all tables through create table query in the database.
    After that, insert data into the table from CSV files through import csv query.
    
### Step 3:: Analysis and data cleaning
    After that, I analyzed data in all tables and I found there are too much messy data.
    In all_sessions and analytics, tables have many duplicate values and NULL values.
    Deleted duplicate data from all tables and tried to make it complete and concise.
    Put average values of data in columns replace them with null values and remove null values.
    Deleted many columns which have null values entire dataset.
    Make data accurate, Completeness, and consistent.

### Step 4:: Add Primary key and Foregine key
    In  all_sessions table removed all duplicate data and make fullvisitorid column of all_session and productsku column of the product table a primary key.
    Use these keys in other tables like analytics, sales_by_Sku, and sales_reports as Foregine keys and to make connections between tables.

### Step 5:: QA Process
    Checked and removed duplicate and NULL values from tables.
    checked all datatypes and altered where need.
    checked the connection between all tables through JOIN query and their unique values.
    
## Results
    I got accurate and complete data from too messy data and run SQL queries to get outputs.
    and the questions result attached into files.

## Challenges 
(1) Problem with importing csv files into PGadmin because of path issues.
(2) Data was too messy and duplicated so it was hard to understand data.
(3) Problem with to created the primary key for a table because the table has many duplicate data.
(4) When I deleted duplicate data from tables I lost all data of tables so I created tables many times.
(5) Problem with connecting foreign key with tables because of some connection issue in PGadmin.


## Future Goals
 I will make data more concise and accurate and to get more accurate result and try to use new function of SQL which will help improve the skills of SQL.


