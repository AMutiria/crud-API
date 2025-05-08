# Library Management System Database

This document outlines the schema for the Library Management System database, designed and implemented using MySQL. This database is intended to manage books, authors, genres, library members, book loans, and reservations.

## Database Schema

The database consists of the following tables:

### 1. `Books`

This table stores information about each book in the library.

| Column Name      | Data Type     | Constraints                      | Description                                           |
|------------------|---------------|----------------------------------|-------------------------------------------------------|
| `BookID`         | `INT`         | `PRIMARY KEY`, `AUTO_INCREMENT`  | Unique identifier for each book.                      |
| `Title`          | `VARCHAR(255)`| `NOT NULL`                       | Title of the book.                                    |
| `ISBN`           | `VARCHAR(20)` | `UNIQUE`, `NOT NULL`             | International Standard Book Number.                   |
| `PublicationYear`| `INT`         |                                  | Year the book was published.                          |
| `AuthorID`       | `INT`         | `FOREIGN KEY`, `REFERENCES Authors(AuthorID)` | Foreign key linking to the `Authors` table.         |
| `GenreID`        | `INT`         | `FOREIGN KEY`, `REFERENCES Genres(GenreID)`   | Foreign key linking to the `Genres` table.          |
| `TotalCopies`    | `INT`         | `NOT NULL`, `DEFAULT 1`          | Total number of copies of this book in the library.  |
| `AvailableCopies`| `INT`         | `NOT NULL`, `DEFAULT 1`          | Number of copies currently available for borrowing. |

### 2. `Authors`

This table stores information about the authors of the books.

| Column Name | Data Type    | Constraints                     | Description                           |
|-------------|--------------|---------------------------------|---------------------------------------|
| `AuthorID`  | `INT`        | `PRIMARY KEY`, `AUTO_INCREMENT` | Unique identifier for each author.    |
| `FirstName` | `VARCHAR(100)`| `NOT NULL`                      | Author's first name.                  |
| `LastName`  | `VARCHAR(100)`| `NOT NULL`                      | Author's last name.                   |

### 3. `Genres`

This table stores the different genres of books.

| Column Name | Data Type   | Constraints                     | Description                      |
|-------------|-------------|---------------------------------|----------------------------------|
| `GenreID`   | `INT`       | `PRIMARY KEY`, `AUTO_INCREMENT` | Unique identifier for each genre. |
| `GenreName` | `VARCHAR(50)`| `UNIQUE`, `NOT NULL`            | Name of the genre.               |

### 4. `Members`

This table stores information about the library members.

| Column Name    | Data Type     | Constraints                      | Description                                  |
|----------------|---------------|----------------------------------|----------------------------------------------|
| `MemberID`     | `INT`         | `PRIMARY KEY`, `AUTO_INCREMENT`  | Unique identifier for each member.           |
| `FirstName`    | `VARCHAR(100)`| `NOT NULL`                       | Member's first name.                         |
| `LastName`     | `VARCHAR(100)`| `NOT NULL`                       | Member's last name.                          |
| `MembershipDate`| `DATE`        | `NOT NULL`                       | Date the member joined the library.          |
| `Email`        | `VARCHAR(100)`| `UNIQUE`                         | Member's email address.                      |
| `Phone`        | `VARCHAR(20)` | `UNIQUE`                         | Member's phone number.                       |

### 5. `Loans`

This table tracks the borrowing and returning of books by members.

| Column Name | Data Type | Constraints                                  | Description                                     |
|-------------|-----------|----------------------------------------------|-------------------------------------------------|
| `LoanID`    | `INT`     | `PRIMARY KEY`, `AUTO_INCREMENT`              | Unique identifier for each loan record.         |
| `BookID`    | `INT`     | `NOT NULL`, `FOREIGN KEY`, `REFERENCES Books(BookID)`   | Foreign key linking to the `Books` table.       |
| `MemberID`  | `INT`     | `NOT NULL`, `FOREIGN KEY`, `REFERENCES Members(MemberID)` | Foreign key linking to the `Members` table.     |
| `LoanDate`  | `DATE`    | `NOT NULL`                                   | Date the book was borrowed.                     |
| `ReturnDate`| `DATE`    |                                              | Date the book was returned (NULL if not returned).|
| `DueDate`   | `DATE`    | `NOT NULL`                                   | Date the book is due to be returned.            |

### 6. `Reservations`

This table tracks book reservations made by members.

| Column Name     | Data Type   | Constraints                                     | Description                                           |
|-----------------|-------------|-------------------------------------------------|-------------------------------------------------------|
| `ReservationID` | `INT`       | `PRIMARY KEY`, `AUTO_INCREMENT`                 | Unique identifier for each reservation.               |
| `BookID`        | `INT`       | `NOT NULL`, `FOREIGN KEY`, `REFERENCES Books(BookID)`   | Foreign key linking to the `Books` table.             |
| `MemberID`      | `INT`       | `NOT NULL`, `FOREIGN KEY`, `REFERENCES Members(MemberID)` | Foreign key linking to the `Members` table.           |
| `ReservationDate`| `TIMESTAMP` | `DEFAULT CURRENT_TIMESTAMP`                     | Timestamp when the reservation was made.              |
|                 |             | `UNIQUE KEY (BookID, MemberID)`                 | Prevents duplicate reservations for the same book/member.|

## Relationships

The database implements the following relationships:

* **One-to-Many:**
    * One Author can have multiple Books.
    * One Genre can have multiple Books.
    * One Member can have multiple Loans.
    * One Book can be involved in multiple Loans.
    * One Member can make multiple Reservations.
    * One Book can have multiple Reservations.

## How to Use

To create this database in your MySQL environment, you can execute the SQL statements provided in the `[YourSQLFileName].sql` file (e.g., `library_schema.sql`).

```bash
mysql -u [your_username] -p < [YourSQLFileName].sql
