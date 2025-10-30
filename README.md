<img src="https://socialify.git.ci/Mluleki23/Library-Management-System-PostgreSQL-/image?language=1&owner=1&name=1&stargazers=1&theme=Light" alt="Library-Management-System-PostgreSQL-" width="640" height="320" />

---

# Library Management System (PostgreSQL)

## Project Overview
This project implements a basic Library Management System using PostgreSQL. It manages books, authors, and patrons, allowing for CRUD operations and advanced queries.

---

## Setup Instructions

### 1. Create the Database
Open `pgAdmin` or `psql` and run:

```sql
CREATE DATABASE LibraryDB;
```

### 2. Connect to the Database
In your SQL client:

```sql
\c LibraryDB
```

### 3. Create Tables

```sql
CREATE TABLE authors (
  author_id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  nationality VARCHAR(100) NOT NULL,
  birth_year INT,
  death_year INT
);

CREATE TABLE books (
  booksId SERIAL PRIMARY KEY,
  title VARCHAR(100) NOT NULL,
  genres VARCHAR(100),
  published_year INT,
  available BOOLEAN,
  author_id INT REFERENCES authors(author_id)
);

CREATE TABLE patrons (
  patronsId SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) NOT NULL,
  borrowed_books INT[] DEFAULT ARRAY[]::INT[]
);
```

---

## Data Insertion

### Insert Sample Authors

```sql
INSERT INTO authors (name, nationality, birth_year, death_year) VALUES
('George Orwell', 'British', 1903, 1950),
('Harper Lee', 'American', 1926, 2016),
('F. Scott Fitzgerald', 'American', 1896, 1940),
('Aldous Huxley', 'British', 1894, 1963),
('J.D. Salinger', 'American', 1919, 2010),
('Herman Melville', 'American', 1819, 1891),
('Jane Austen', 'British', 1775, 1817),
('Leo Tolstoy', 'Russian', 1828, 1910),
('Fyodor Dostoevsky', 'Russian', 1821, 1881),
('J.R.R. Tolkien', 'British', 1892, 1973);
```

### Insert Sample Books

```sql
INSERT INTO books (title, genres, published_year, available, author_id) VALUES
('1984', 'Dystopian, Political Fiction', 1949, TRUE, 1),
('To Kill a Mockingbird', 'Southern Gothic, Bildungsroman', 1960, TRUE, 2),
('The Great Gatsby', 'Tragedy', 1925, TRUE, 3),
('Brave New World', 'Dystopian, Science Fiction', 1932, TRUE, 4),
('The Catcher in the Rye', 'Realist, Bildungsroman', 1951, TRUE, 5),
('Moby-Dick', 'Adventure Fiction', 1851, TRUE, 6),
('Pride and Prejudice', 'Romantic Novel', 1813, TRUE, 7),
('War and Peace', 'Historical Novel', 1869, TRUE, 8),
('Crime and Punishment', 'Philosophical Novel', 1866, TRUE, 9),
('The Hobbit', 'Fantasy', 1937, TRUE, 10);
```

### Insert Sample Patrons

```sql
INSERT INTO patrons (name, email, borrowed_books) VALUES
('Alice Johnson', 'alice@example.com', ARRAY[]::INT[]),
('Bob Smith', 'bob@example.com', ARRAY[1, 2]),
('Carol White', 'carol@example.com', ARRAY[]::INT[]),
('David Brown', 'david@example.com', ARRAY[3]),
('Eve Davis', 'eve@example.com', ARRAY[]::INT[]),
('Frank Moore', 'frank@example.com', ARRAY[4, 5]),
('Grace Miller', 'grace@example.com', ARRAY[]::INT[]),
('Hank Wilson', 'hank@example.com', ARRAY[6]),
('Ivy Taylor', 'ivy@example.com', ARRAY[]::INT[]),
('Jack Anderson', 'jack@example.com', ARRAY[7, 8]);
```

---

## Basic Operations

### 1. Get all books

```sql
SELECT * FROM books;
```

### 2. Get a book by title

```sql
SELECT * FROM books WHERE title = '1984';
```

### 3. Get all books by a specific author (e.g., J.R.R. Tolkien)

```sql
SELECT b.* FROM books b
JOIN authors a ON b.author_id = a.author_id
WHERE a.name = 'J.R.R. Tolkien';
```

### 4. Get all available books

```sql
SELECT * FROM books WHERE available = TRUE;
```

---

## Update Operations

### 1. Mark a book as borrowed (set available = FALSE)

```sql
UPDATE books SET available = FALSE WHERE booksId = 1; -- '1984'
```

### 2. Add a new genre to an existing book

```sql
UPDATE books SET genres = genres || ', Classic' WHERE booksId = 2; -- 'To Kill a Mockingbird'
```

> **Note:** Since `genres` is a VARCHAR, this concatenates strings. If `genres` was converted to an array, use `array_append`.

### 3. Add a borrowed book to a patron’s record

```sql
UPDATE patrons SET borrowed_books = array_append(borrowed_books, 3) WHERE patronsId = 4; -- David Brown
```

---

## Delete Operations

### 1. Delete a book by title

```sql
DELETE FROM books WHERE title = 'The Hobbit';
```

### 2. Delete an author by ID

```sql
DELETE FROM authors WHERE author_id = 10; -- J.R.R. Tolkien
```

---

## Advanced Queries

### 1. Find books published after 1950

```sql
SELECT * FROM books WHERE published_year > 1950;
```

### 2. Find all American authors

```sql
SELECT * FROM authors WHERE nationality = 'American';
```

### 3. Set all books as available

```sql
UPDATE books SET available = TRUE;
```

### 4. Find all books that are available AND published after 1950

```sql
SELECT * FROM books WHERE available = TRUE AND published_year > 1950;
```

### 5. Find authors whose names contain "George"

```sql
SELECT * FROM authors WHERE name ILIKE '%George%';
```

### 6. Increment the published year 1869 by 1

```sql
UPDATE books SET published_year = published_year + 1 WHERE published_year = 1869;
```

---
