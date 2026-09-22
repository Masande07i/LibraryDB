# LibraryDB

## Sprint 3: Read Operations (Queries)

//CREATE TABLES
CREATE TABLE IF NOT EXISTS authors(
  author_id SERIAL PRIMARY KEY,
  name VARCHAR(50) NOT NULL,
  nationality VARCHAR(50) NOT NULL,
  birth_year INT NOT NULL,
  death_year INT NOT NULL
);

CREATE TABLE IF NOT EXISTS books (
  book_id SERIAL PRIMARY KEY,
  title VARCHAR(50) NOT NULL,
  author_id INT REFERENCES authors(author_id),
  genres TEXT[],
  published_year INT,
  available BOOLEAN
); 

CREATE TABLE IF NOT EXISTS patrons(
  patrons_id SERIAL PRIMARY KEY,
  name VARCHAR(50) NOT NULL,
  email VARCHAR(50) NOT NULL,
  borrowed_books TEXT[]
);

//SELECT ALL
SELECT * FROM books;

//Get a book by title
SELECT title FROM books;
SELECT title FROM books WHERE title= "title name"

//Get all books by a specific author.
SELECT * FROM books WHERE author_id = 3;

//Get all available books
SELECT * FROM books WHERE available = true;

## Sprint 4: Update Operations


//Mark a book as borrowed (set available = false)
UPDATE books
SET available = false
WHERE book_id = 1

//Add a new genre to an existing book

UPDATE books 
SET genres = genres || ARRAY['Novel'] 
WHERE book_id = 3;

# Sprint 5: Delete Operations
//Delete a book by title.
DELETE books WHERE title = "";


//Delete an author by ID.
DELETE authors WHERE author_id = 

## Sprint 6: Advanced Queries

//Find books published after 1950
SELECT * FROM books WHERE published_year > 1950;

//Find all American authors.
SELECT * FROM authors WHERE nationality = 'American';

//Find authors whose names contain "George".
SELECT * FROM authors WHERE name LIKE '%George%';

//Increment the published year 1869 by 1.
UPDATE books
SET published_year = published_year + 1
WHERE published_year = 1869;
