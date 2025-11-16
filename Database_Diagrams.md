# Database Diagrams

This file contains database diagrams and schemas created using [Eraser.io](https://eraser.io)

---

## Library Management System

**Created on**: November 16, 2025

**Description**: A complete database schema for managing a library system including books, members, loans, authors, and staff.

**Eraser.io Link**: [Paste your Eraser.io diagram link here]

---

### Database Schema Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                  LIBRARY MANAGEMENT SYSTEM                      │
└─────────────────────────────────────────────────────────────────┘
```

---

### Tables and Relationships

#### 1. **MEMBERS Table**
Stores information about library members.

| Column Name    | Data Type    | Constraints           | Description                    |
|----------------|--------------|------------------------|--------------------------------|
| member_id      | INT          | PRIMARY KEY, AUTO_INC | Unique member identifier       |
| first_name     | VARCHAR(50)  | NOT NULL              | Member's first name            |
| last_name      | VARCHAR(50)  | NOT NULL              | Member's last name             |
| email          | VARCHAR(100) | UNIQUE, NOT NULL      | Member's email address         |
| phone          | VARCHAR(15)  |                        | Contact number                 |
| address        | TEXT         |                        | Home address                   |
| membership_date| DATE         | NOT NULL              | Date of membership             |
| membership_type| VARCHAR(20)  | NOT NULL              | Standard/Premium/Student       |
| status         | VARCHAR(20)  | DEFAULT 'Active'      | Active/Suspended/Expired       |

---

#### 2. **BOOKS Table**
Stores information about books in the library.

| Column Name    | Data Type    | Constraints           | Description                    |
|----------------|--------------|------------------------|--------------------------------|
| book_id        | INT          | PRIMARY KEY, AUTO_INC | Unique book identifier         |
| isbn           | VARCHAR(13)  | UNIQUE, NOT NULL      | International Standard Book No.|
| title          | VARCHAR(200) | NOT NULL              | Book title                     |
| publisher      | VARCHAR(100) |                        | Publisher name                 |
| publication_year| INT         |                        | Year of publication            |
| category       | VARCHAR(50)  |                        | Fiction/Non-fiction/Science etc|
| total_copies   | INT          | NOT NULL, DEFAULT 1   | Total copies available         |
| available_copies| INT         | NOT NULL, DEFAULT 1   | Currently available copies     |
| shelf_location | VARCHAR(20)  |                        | Physical location in library   |
| price          | DECIMAL(10,2)|                        | Book price                     |

---

#### 3. **AUTHORS Table**
Stores information about book authors.

| Column Name    | Data Type    | Constraints           | Description                    |
|----------------|--------------|------------------------|--------------------------------|
| author_id      | INT          | PRIMARY KEY, AUTO_INC | Unique author identifier       |
| first_name     | VARCHAR(50)  | NOT NULL              | Author's first name            |
| last_name      | VARCHAR(50)  | NOT NULL              | Author's last name             |
| biography      | TEXT         |                        | Author's biography             |
| nationality    | VARCHAR(50)  |                        | Author's nationality           |

---

#### 4. **BOOK_AUTHORS Table** (Junction Table)
Many-to-Many relationship between Books and Authors.

| Column Name    | Data Type    | Constraints           | Description                    |
|----------------|--------------|------------------------|--------------------------------|
| book_author_id | INT          | PRIMARY KEY, AUTO_INC | Unique identifier              |
| book_id        | INT          | FOREIGN KEY, NOT NULL | References BOOKS(book_id)      |
| author_id      | INT          | FOREIGN KEY, NOT NULL | References AUTHORS(author_id)  |

---

#### 5. **LOANS Table**
Tracks book borrowing transactions.

| Column Name    | Data Type    | Constraints           | Description                    |
|----------------|--------------|------------------------|--------------------------------|
| loan_id        | INT          | PRIMARY KEY, AUTO_INC | Unique loan identifier         |
| book_id        | INT          | FOREIGN KEY, NOT NULL | References BOOKS(book_id)      |
| member_id      | INT          | FOREIGN KEY, NOT NULL | References MEMBERS(member_id)  |
| staff_id       | INT          | FOREIGN KEY           | References STAFF(staff_id)     |
| loan_date      | DATE         | NOT NULL              | Date book was borrowed         |
| due_date       | DATE         | NOT NULL              | Date book should be returned   |
| return_date    | DATE         |                        | Actual return date (NULL if not returned)|
| status         | VARCHAR(20)  | DEFAULT 'Active'      | Active/Returned/Overdue        |
| fine_amount    | DECIMAL(10,2)| DEFAULT 0.00          | Fine for late return           |

---

#### 6. **STAFF Table**
Stores information about library staff members.

| Column Name    | Data Type    | Constraints           | Description                    |
|----------------|--------------|------------------------|--------------------------------|
| staff_id       | INT          | PRIMARY KEY, AUTO_INC | Unique staff identifier        |
| first_name     | VARCHAR(50)  | NOT NULL              | Staff's first name             |
| last_name      | VARCHAR(50)  | NOT NULL              | Staff's last name              |
| email          | VARCHAR(100) | UNIQUE, NOT NULL      | Staff email                    |
| phone          | VARCHAR(15)  |                        | Contact number                 |
| position       | VARCHAR(50)  | NOT NULL              | Librarian/Assistant/Manager    |
| hire_date      | DATE         | NOT NULL              | Date of joining                |
| salary         | DECIMAL(10,2)|                        | Monthly salary                 |

---

#### 7. **RESERVATIONS Table**
Tracks book reservations when books are unavailable.

| Column Name    | Data Type    | Constraints           | Description                    |
|----------------|--------------|------------------------|--------------------------------|
| reservation_id | INT          | PRIMARY KEY, AUTO_INC | Unique reservation identifier  |
| book_id        | INT          | FOREIGN KEY, NOT NULL | References BOOKS(book_id)      |
| member_id      | INT          | FOREIGN KEY, NOT NULL | References MEMBERS(member_id)  |
| reservation_date| DATE        | NOT NULL              | Date of reservation            |
| status         | VARCHAR(20)  | DEFAULT 'Pending'     | Pending/Fulfilled/Cancelled    |
| expiry_date    | DATE         |                        | Reservation expiry date        |

---

#### 8. **FINES Table**
Tracks fines and payments.

| Column Name    | Data Type    | Constraints           | Description                    |
|----------------|--------------|------------------------|--------------------------------|
| fine_id        | INT          | PRIMARY KEY, AUTO_INC | Unique fine identifier         |
| loan_id        | INT          | FOREIGN KEY, NOT NULL | References LOANS(loan_id)      |
| member_id      | INT          | FOREIGN KEY, NOT NULL | References MEMBERS(member_id)  |
| fine_amount    | DECIMAL(10,2)| NOT NULL              | Amount of fine                 |
| paid_amount    | DECIMAL(10,2)| DEFAULT 0.00          | Amount paid                    |
| fine_date      | DATE         | NOT NULL              | Date fine was issued           |
| payment_date   | DATE         |                        | Date fine was paid             |
| status         | VARCHAR(20)  | DEFAULT 'Unpaid'      | Unpaid/Paid/Waived             |

---

### Relationships Diagram

```
                    ┌─────────────┐
                    │   AUTHORS   │
                    │-------------|
                    │ author_id   │ PK
                    │ first_name  │
                    │ last_name   │
                    └──────┬──────┘
                           │
                           │ 1:N
                           │
                    ┌──────▼──────────┐
                    │ BOOK_AUTHORS    │  (Junction Table)
                    │-----------------|
                    │ book_author_id  │ PK
                    │ book_id         │ FK
                    │ author_id       │ FK
                    └──────┬──────────┘
                           │
                           │ N:1
                           │
    ┌─────────────┐   ┌────▼──────────┐   ┌──────────────┐
    │ RESERVATIONS│   │    BOOKS      │   │    LOANS     │
    │-------------|   │---------------|   │--------------|
    │reservation_id│  │ book_id       │PK │ loan_id      │ PK
    │ book_id     │FK─┤ isbn          │   │ book_id      │ FK──┐
    │ member_id   │FK │ title         │   │ member_id    │ FK──┤
    └─────────────┘   │ total_copies  │   │ staff_id     │ FK  │
                      │available_copies│  │ loan_date    │     │
                      └───────────────┘   │ due_date     │     │
                                          │ return_date  │     │
                                          │ status       │     │
                                          └──────┬───────┘     │
                                                 │             │
                                                 │ 1:N         │
                                                 │             │
                    ┌────────────┐        ┌──────▼─────────┐   │
                    │   STAFF    │        │     FINES      │   │
                    │------------|        │----------------|   │
                    │ staff_id   │PK      │ fine_id        │PK │
                    │ first_name │        │ loan_id        │FK─┘
                    │ last_name  │        │ member_id      │FK─┐
                    │ position   │        │ fine_amount    │   │
                    └────────────┘        │ status         │   │
                                          └────────────────┘   │
                                                               │
                                                               │
                                          ┌────────────────────┘
                                          │
                                    ┌─────▼──────┐
                                    │  MEMBERS   │
                                    │------------|
                                    │ member_id  │ PK
                                    │ first_name │
                                    │ last_name  │
                                    │ email      │
                                    │ status     │
                                    └────────────┘
```

---

### Key Relationships

1. **MEMBERS ↔ LOANS** (1:N)
   - One member can have multiple loans
   - Each loan belongs to one member

2. **BOOKS ↔ LOANS** (1:N)
   - One book can be loaned multiple times (across different time periods)
   - Each loan is for one book

3. **STAFF ↔ LOANS** (1:N)
   - One staff member can process multiple loans
   - Each loan is processed by one staff member

4. **BOOKS ↔ AUTHORS** (M:N)
   - One book can have multiple authors
   - One author can write multiple books
   - Implemented through BOOK_AUTHORS junction table

5. **MEMBERS ↔ RESERVATIONS** (1:N)
   - One member can make multiple reservations
   - Each reservation belongs to one member

6. **BOOKS ↔ RESERVATIONS** (1:N)
   - One book can have multiple reservations
   - Each reservation is for one book

7. **LOANS ↔ FINES** (1:1 or 1:N)
   - One loan can have one fine
   - Each fine is associated with one loan

8. **MEMBERS ↔ FINES** (1:N)
   - One member can have multiple fines
   - Each fine belongs to one member

---

### Database Workflow

#### **Borrowing a Book:**
```
1. Member requests book → Check MEMBERS table (status = Active)
2. Check BOOKS table → available_copies > 0
3. Insert record in LOANS table
4. Update BOOKS.available_copies (decrease by 1)
5. Set loan_date and due_date
6. Status = 'Active'
```

#### **Returning a Book:**
```
1. Update LOANS.return_date
2. Update LOANS.status = 'Returned'
3. Update BOOKS.available_copies (increase by 1)
4. Check if return_date > due_date
   - If YES: Calculate fine and insert into FINES table
   - If NO: No action
5. Check RESERVATIONS for pending reservations
   - Notify next member in queue
```

#### **Book Reservation:**
```
1. Member requests unavailable book
2. Check BOOKS.available_copies = 0
3. Insert record in RESERVATIONS table
4. Status = 'Pending'
5. When book available → Update status = 'Fulfilled'
6. Notify member
```

#### **Fine Calculation:**
```
Fine Amount = (return_date - due_date) × daily_fine_rate
- Example: $2 per day overdue
- Maximum fine cap: $50
```

---

### Indexes for Performance

```sql
-- Speed up searches
CREATE INDEX idx_books_title ON BOOKS(title);
CREATE INDEX idx_books_isbn ON BOOKS(isbn);
CREATE INDEX idx_members_email ON MEMBERS(email);
CREATE INDEX idx_loans_status ON LOANS(status);
CREATE INDEX idx_loans_due_date ON LOANS(due_date);
```

---

### Sample Queries

#### 1. Find all books currently on loan
```sql
SELECT b.title, b.isbn, m.first_name, m.last_name, l.due_date
FROM LOANS l
JOIN BOOKS b ON l.book_id = b.book_id
JOIN MEMBERS m ON l.member_id = m.member_id
WHERE l.status = 'Active';
```

#### 2. Find overdue books
```sql
SELECT b.title, m.first_name, m.last_name, l.due_date, 
       DATEDIFF(CURDATE(), l.due_date) AS days_overdue
FROM LOANS l
JOIN BOOKS b ON l.book_id = b.book_id
JOIN MEMBERS m ON l.member_id = m.member_id
WHERE l.status = 'Active' AND l.due_date < CURDATE();
```

#### 3. Find all books by a specific author
```sql
SELECT b.title, b.isbn, b.publication_year
FROM BOOKS b
JOIN BOOK_AUTHORS ba ON b.book_id = ba.book_id
JOIN AUTHORS a ON ba.author_id = a.author_id
WHERE a.first_name = 'J.K.' AND a.last_name = 'Rowling';
```

#### 4. Get member borrowing history
```sql
SELECT b.title, l.loan_date, l.return_date, l.status
FROM LOANS l
JOIN BOOKS b ON l.book_id = b.book_id
WHERE l.member_id = 123
ORDER BY l.loan_date DESC;
```

#### 5. Find members with unpaid fines
```sql
SELECT m.first_name, m.last_name, SUM(f.fine_amount - f.paid_amount) AS total_due
FROM FINES f
JOIN MEMBERS m ON f.member_id = m.member_id
WHERE f.status = 'Unpaid'
GROUP BY m.member_id;
```

---

### Business Rules

1. **Loan Period**: Standard loan period is 14 days
2. **Maximum Loans**: Members can borrow up to 5 books at a time
3. **Renewal**: Books can be renewed once if no reservations exist
4. **Fine Rate**: $2 per day for overdue books
5. **Reservation Expiry**: Reservations expire after 3 days if not collected
6. **Membership Types**:
   - Standard: 5 books, 14 days
   - Premium: 10 books, 30 days
   - Student: 3 books, 14 days

---

### Notes
- Use transactions when updating multiple tables (e.g., borrowing/returning books)
- Implement triggers to automatically update available_copies
- Add audit logs for tracking all database changes
- Implement soft deletes (mark as deleted rather than actually deleting records)
- Regular backups scheduled daily
- Archive old loan records yearly

---

*Last Updated: November 16, 2025*
