## What is a Database?

A **database** is a shared collection of interrelated data designed to serve the needs of multiple users and applications.

**Key characteristics:**
- **Shared Resource** – Multiple users and programs can access it simultaneously.
- **Data Independence** – The data is stored in a way that is separate from the programs that use it.
- **Multiple Views** – Different users may see the data in different formats or perspectives.

**Why Databases Matter:**
- Allow **organized storage** of information.
- Enable **efficient retrieval and updates**.
- Protect data from **loss** and **unauthorized access**.
- Facilitate **analysis and decision-making**.

**Example:**
Imagine an online bookstore’s database:
- **Tables** for `Books`, `Authors`, `Orders`, and `Customers`.
- Customers can search for books by title, author, or genre.
- Staff can update stock levels and process orders.
- Management can analyze sales trends.

---

## Why Use a Database?

Databases provide a structured, efficient, and secure way to store and manage data.  

**Main benefits include:**

- **Easily Accessed** – Retrieve data quickly using queries.
- **Easily Modified** – Update, delete, or insert new records without disrupting other data.
- **Protected** – Control access using authentication, authorization, and encryption.
- **Analyzed** – Generate reports, run analytics, and support informed decision-making.

**Example:**
A hospital database can instantly retrieve a patient's medical history, update new test results, and ensure that only authorized staff can access sensitive records.

---

## The Database Approach

The database approach treats data as a valuable organizational asset that must be carefully managed and maintained.

**Key ideas:**
- Data is considered a **shared resource**, just like people, equipment, or money.
- Centralized control reduces data redundancy and inconsistency.
- Databases support **business operations** as well as **decision-making processes**.

**Advantages:**
- Consistent, up-to-date information for all users.
- Easier integration of data from different departments.
- Improved data quality and accuracy.

**Example:**
In a retail company, sales, inventory, and customer data can be integrated into a single database, ensuring all departments work with the same accurate information.

---

## Evolution of Database Systems

Database systems have evolved over decades to handle growing volumes, variety, and complexity of data.

**Key stages:**
1. **File-Based Systems** – Early approach using separate files for each application, leading to redundancy and inconsistency.
2. **Hierarchical & Network Databases** – Structured in tree or graph form for faster navigation, but inflexible.
3. **Relational Databases (RDBMS)** – Organize data into tables with rows and columns; data accessed using SQL.
4. **Object-Oriented Databases** – Store complex data types like multimedia alongside traditional data.
5. **NoSQL & NewSQL Databases** – Designed for scalability, unstructured data, and distributed environments.
6. **Cloud Databases** – Hosted and managed on cloud infrastructure, offering on-demand scalability and global access.

**Example:**
A social media platform may combine relational databases for user accounts, NoSQL for storing posts and comments, and cloud databases for global availability.

---

## Relational Database Example

A **relational database** organizes data into tables (relations) consisting of rows (records) and columns (attributes).  
Relationships between tables are established through **primary keys** and **foreign keys**.

**Key features:**
- **Tables** store related data in a structured way.
- **Primary Key** uniquely identifies each row in a table.
- **Foreign Key** links one table to another, ensuring referential integrity.
- **SQL** (Structured Query Language) is used to query and manipulate data.

**Example:**
Consider an online bookstore:
- `Books` table: stores ISBN, title, author ID, price.
- `Authors` table: stores author ID, name, country.
- `Orders` table: stores order ID, customer ID, date, total amount.
- Relationships:
  - Each book is linked to an author through `author_id`.
  - Each order links to multiple books through an order-details table.

```sql
SELECT title, name
FROM Books
JOIN Authors ON Books.author_id = Authors.author_id;
```

This query retrieves each book’s title along with its author’s name.

---

