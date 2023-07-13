---
title: "Introduction To Relational Databases With PostgreSQL."
seoDescription: "A Database is an organized collection of data stored and accessed electronically from a computer system. Where databases are more complex they are often..."
datePublished: Sat Mar 26 2022 13:50:52 GMT+0000 (Coordinated Universal Time)
cuid: cl17wuak201hzsenv4kdlho43
slug: introduction-to-relational-databases-with-postgresql
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1650274405940/ZS-zXiN64.png
tags: introduction, postgresql, data, databases, sql

---

# SQL Tutorial (Postgres)

## What is a Database?

A Database is an organized collection of data stored and accessed electronically from a computer system. Where databases are more complex they are often developed using formal design and modeling techniques.

## What is DBMS?

The database management system (DBMS) is the software that interacts with end-users, applications, and the database itself to capture and analyze the data. The DBMS software additionally encompasses the core facilities provided to administer the database. The sum total of the database, the DBMS, and the associated applications can be referred to as a "database system". Often the term "database" is also used loosely to refer to any of the DBMS, the database system, or an application associated with the database.

## What is SQL?

**SQL stands for Structured Query Language ( Pronounced as "sequel"  )** which is a language used by databases. This language allows to handle the information using tables and shows a language to query these tables. Most of the databases management systems like SQL Server, Oracle, PostgreSQL, MySQL, MariaDB use this language to handle the data. With SQL you can insert, delete, and update data. You can also create, delete, or alter database objects.

## What is RDBMS?

A Relational Database Management System (RDBMS) is a type of database management system (DBMS) that stores data in a row-based table structure that connects related data elements. An RDBMS includes functions that maintain the security, accuracy, integrity, and consistency of the data. This is different than the file storage used in a DBMS.

## Some Popular RDBMS

- PostgreSQL
- MySQL
- Oracle
- SQL Server
- IBM DB2
- Microsoft Access
- SQLite
- MariaDB
- Informix
- Azure SQL

## What Is NoSQL Database?

A NoSQL (originally referring to "non-SQL" or "non-relational") database provides a mechanism for storage and retrieval of data that is modeled in means other than the tabular relations used in relational databases.

## Types Of NoSQL Databases

- Document
    - MongoDB
    - Firestore
    - DynamoDB
- Key-Value
    - Redis
    - Riak
    - Oracle NoSQL
- Graph
    - Neo4J
    - Amazon Neptune
    - Allegro Graph
- Wide column
    - Apache HBase
    - Google BigTable
    - Microsoft Azure Cosmos DB

## Why PostgreSQL?

PostgreSQL comes with many features aimed to help developers build applications, administrators to protect data integrity and build fault-tolerant environments, and help you manage your data no matter how big or small the dataset. In addition to being free and open-source, PostgreSQL is highly extensible. For example, you can define your own data types, build out custom functions, even write code from different programming languages without recompiling your database!

## Structure Of SQL Databases

---

## Courses (Table)
| ID | Title                      | Price |
|----|----------------------------|-------|
| 1  | Learn SQL From Scratch     | 50    |
| 2  | Learn C# From Scratch      | 70    |
| 3  | Learn EF Core From Scratch | 40    |

## Interacting With SQL Databases

1. CLI
2. GUI
3. ORM
4. SQL Editor

### CLI ( Command Line Interface )

SQL Command Line (*PSQL In Postgres)* is a command-line tool for accessing PostgreSQL Database. It enables you to enter and run SQL Commands Like SELECT, INSERT, ALTER

### GUI ( Graphical User Interface )

PGAdmin is a web-based GUI tool used to interact with the Postgres database sessions, both locally and remote servers as well. You can use PGAdmin to perform any sort of database administration required for a Postgres database

### ORM ( Object Relational Mapping )

ORM is a programming technique for converting data between incompatible type systems using object-oriented programming languages. This creates, in effect, a "virtual object database" that can be used from within the programming language.

### ORM For Popular Programming Languages

- JavaScript
    - Sequelize
    - Prisma
- TypeScript
    - TypeORM
    - Mikro-ORM
- Python
    - SQLAlchemy
    - Django ORM

### SQL Editor

SQL editor is like a command-line interface but it has more features like autosuggestions and syntax highlighting that make you more productive with SQL, you can also save your connections and queries in SQL editor.

## Creating Database

```bash
# With Default User
createdb database_name
# With Custom User
createdb -U username database_name
```

---

# SQL Queries

## [SELECT](https://www.postgresql.org/docs/9.2/sql-select.html) - Selecting Data

Simple Usage:

```sql
SELECT values_to_select FROM table_name;
```

With Condition: 

```sql
SELECT values_to_select FROM table_name WHERE column_name = value_to_check;
```

With Limit:

```sql
SELECT values_to_select FROM table_name LIMIT number_of_rows;
```

## [CREATE](https://www.postgresql.org/docs/9.2/sql-createtype.html) - Creating Data

Simple Usage:

```sql
CREATE type name;
```

Creating Database:

```sql
CREATE DATABASE database_name;
```

Creating Table:

```sql
CREATE TABLE table_name (
	column_1 type_of_column,
	column_2 type_of_column,
	column_3 type_of_column,
);
```
