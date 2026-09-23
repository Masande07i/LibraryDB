# Library Management System - PostgreSQL

## Project image

<img src="https://socialify.git.ci/Masande07i/LibraryDB/image?language=1&owner=1&name=1&stargazers=1&theme=Light" alt="LibraryDB" width="640" height="320" />

## Project Description

This project is a Library Management System built using PostgreSQL.

The system manages:

* Books
* Authors
* Patrons
* Book availability
* Borrowed books

The project demonstrates basic and advanced SQL operations including creating tables, inserting data, retrieving records, updating records, deleting records, filtering, searching, and working with relationships using foreign keys.

---

# Database

Database name:

```
LibraryDB
```

The database contains three main tables:

* `authors`
* `books`
* `patrons`

The `books` table has a foreign key relationship with the `authors` table.

```
Authors
   
    author_id
   
Books
```

---

# Sprint 1: Project Setup

## 1. Create the Database

Create a database called:

```
CREATE DATABASE LibraryDB;
```

After creating the database, connect to `LibraryDB` before creating the tables.

---

## 2. Create Authors Table

```sql
CREATE TABLE IF NOT EXISTS authors (
    author_id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    nationality VARCHAR(50) NOT NULL,
    birth_year INT NOT NULL,
    death_year INT NOT NULL
);
```

---

## 3. Create Books Table

The `author_id` column is a foreign key that connects each book to an author.

```sql
CREATE TABLE IF NOT EXISTS books (
    book_id SERIAL PRIMARY KEY,
    title VARCHAR(50) NOT NULL,
    author_id INT REFERENCES authors(author_id),
    genres TEXT[],
    published_year INT,
    available BOOLEAN
);
```

---

## 4. Create Patrons Table

The `borrowed_books` column stores the IDs of books borrowed by each patron.

```sql
CREATE TABLE IF NOT EXISTS patrons (
    patron_id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(50) NOT NULL,
    borrowed_books INT[]
);
```

---

# Sprint 2: Insert Data

## Insert Authors

```sql
INSERT INTO authors (author_id, name, nationality, birth_year, death_year)
VALUES
(1, 'George Orwell', 'British', 1903, 1950),
(2, 'Harper Lee', 'American', 1926, 2016),
(3, 'F. Scott Fitzgerald', 'American', 1896, 1940),
(4, 'Aldous Huxley', 'British', 1894, 1963),
(5, 'J.D. Salinger', 'American', 1919, 2010),
(6, 'Herman Melville', 'American', 1819, 1891),
(7, 'Jane Austen', 'British', 1775, 1817),
(8, 'Leo Tolstoy', 'Russian', 1828, 1910),
(9, 'Fyodor Dostoevsky', 'Russian', 1821, 1881),
(10, 'J.R.R. Tolkien', 'British', 1892, 1973);
```

## Insert Books

```sql
INSERT INTO books (book_id, title, author_id, genres, published_year, available)
VALUES
(1, '1984', 1, ARRAY['Dystopian', 'Political Fiction'], 1949, TRUE),
(2, 'To Kill a Mockingbird', 2, ARRAY['Southern Gothic', 'Bildungsroman'], 1960, TRUE),
(3, 'The Great Gatsby', 3, ARRAY['Tragedy'], 1925, TRUE),
(4, 'Brave New World', 4, ARRAY['Dystopian', 'Science Fiction'], 1932, TRUE),
(5, 'The Catcher in the Rye', 5, ARRAY['Realist Novel', 'Bildungsroman'], 1951, TRUE),
(6, 'Moby-Dick', 6, ARRAY['Adventure Fiction'], 1851, TRUE),
(7, 'Pride and Prejudice', 7, ARRAY['Romantic Novel'], 1813, TRUE),
(8, 'War and Peace', 8, ARRAY['Historical Novel'], 1869, TRUE),
(9, 'Crime and Punishment', 9, ARRAY['Philosophical Novel'], 1866, TRUE),
(10, 'The Hobbit', 10, ARRAY['Fantasy'], 1937, TRUE);
```

## Insert Patrons

```sql
INSERT INTO patrons (patrons_id, name, email, borrowed_books)
VALUES
(1, 'Alice Johnson', 'alice@example.com', ARRAY[]::INT[]),
(2, 'Bob Smith', 'bob@example.com', ARRAY[1, 2]),
(3, 'Carol White', 'carol@example.com', ARRAY[]::INT[]),
(4, 'David Brown', 'david@example.com', ARRAY[3]),
(5, 'Eve Davis', 'eve@example.com', ARRAY[]::INT[]),
(6, 'Frank Moore', 'frank@example.com', ARRAY[4, 5]),
(7, 'Grace Miller', 'grace@example.com', ARRAY[]::INT[]),
(8, 'Hank Wilson', 'hank@example.com', ARRAY[6]),
(9, 'Ivy Taylor', 'ivy@example.com', ARRAY[]::INT[]),
(10, 'Jack Anderson', 'jack@example.com', ARRAY[7, 8]);
```

---

# Sprint 3: Read Operations

## Get All Books

```sql
SELECT * FROM books;
```

## Get a Book by Title

```sql
SELECT * FROM books
WHERE title = '1984';
```

## Get All Books by a Specific Author

Using the author's ID:

```sql
SELECT * FROM books
WHERE author_id = 3;
```

## Get All Available Books

```sql
SELECT * FROM books
WHERE available = TRUE;
```

---

# Sprint 4: Update Operations

## Mark a Book as Borrowed

Set the book's availability to `FALSE` using id.

```sql
UPDATE books
SET available = FALSE
WHERE book_id = 1;
```
## Add a New Genre to an existing book

```sql
UPDATE books
SET genres = genres || ARRAY['Novel']
WHERE book_id = 3;
```

## Add a Borrowed Book to a Patron

For example, add book `3` to patron `2`:

```sql
UPDATE patrons
SET borrowed_books = array_append(borrowed_books, 3)
WHERE patron_id = 2;
```

---

# Sprint 5: Delete Operations

## Delete a Book by Title

```sql
DELETE FROM books
WHERE title = '1984';
```

## Delete an Author by ID

Because books reference authors through a foreign key, an author cannot be deleted while books still reference that author.

First delete the author's books:

```sql
DELETE FROM books
WHERE author_id = 2;
```

Then delete the author:

```sql
DELETE FROM authors
WHERE author_id = 2;
```

---

# Sprint 6: Advanced Queries

## Find Books Published After 1950

```sql
SELECT * FROM books
WHERE published_year > 1950;
```

## Find All American Authors

```sql
SELECT * FROM authors
WHERE nationality = 'American';
```

## Set All Books as Available

```sql
UPDATE books
SET available = TRUE;
```

## Find Available Books Published After 1950

```sql
SELECT * FROM books
WHERE available = TRUE
AND published_year > 1950;
```

## Find Authors Whose Names Contain "George"

```sql
SELECT * FROM authors
WHERE name LIKE '%George%';
```

## Increment the Published Year 1869 by 1

```sql
UPDATE books
SET published_year = published_year + 1
WHERE published_year = 1869;
```

---

# Running the Project in pgAdmin

## Step 1: Open pgAdmin

Open pgAdmin and connect to your PostgreSQL server.

## Step 2: Create the Database

Right-click **Databases** and select:

```
Create → Database
```

Name the database:

```
LibraryDB
```

Click **Save**.

## Step 3: Open Query Tool

Select the `LibraryDB` database.

Right-click the database and select:

```
Query Tool
```

## Step 4: Run the SQL

Copy the SQL commands into the Query Tool.

Run the commands using the **Execute** button or press:

```
F5
```

Make sure you create the tables in this order:

```
1. authors
2. books
3. patrons
```

This is important because `books.author_id` references `authors.author_id`.

## Step 5: View the Tables

In the pgAdmin browser, expand:

```
LibraryDB
    → Schemas
        → public
            → Tables
```

You should see:

```
authors
books
patrons
```

You can right-click a table and select:

```
View/Edit Data → All Rows
```

to view the records.

---






