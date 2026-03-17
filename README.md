# Bank Management CLI

A terminal-based banking system built with Java 17 and pure JDBC,
demonstrating backend fundamentals without ORM abstraction.

![Java](https://img.shields.io/badge/Java-17-orange)
![MySQL](https://img.shields.io/badge/MySQL-8.0-blue)
![JDBC](https://img.shields.io/badge/Persistence-JDBC-green)

## Technologies & Concepts

- **Language:** Java 17 (Switch Expressions, Optionals, Text Blocks)
- **Database:** MySQL 8.0 with integrity constraints (`UNIQUE`, `CHECK`, `FOREIGN KEY`)
- **Persistence:** Pure JDBC for full control over transactions and queries
- **Architecture:** DAO (Data Access Object) pattern for data layer isolation

---

## Key Features

### Customer Management

- **Full CRUD Flow:** Listing, individual search, creation and updates
- **Soft Delete:** Customers are deactivated (`active = false`) instead of deleted, preserving financial history
- **Data Security:** Email validation with `SQLIntegrityConstraintViolationException` handling

### Financial Operations

- **Deposits & Withdrawals:** Centralized processing with strict balance validation
- **Database Logic:** Dynamic balance calculation using `SUM` with `CASE WHEN` directly in SQL for performance
- **Statements:** Transaction history filtered by customer and sorted chronologically

---

## Technical Highlights

- **Null-Safe:** Systematic use of `Optional<Customer>` for lookups, eliminating `NullPointerException` risks
- **Layered Validation:** Business rules verified both in Java and via database constraints
- **Security:** Environment variables for database credentials following Twelve-Factor App best practices

---

## Data Model
```sql
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE,
    active BOOLEAN DEFAULT TRUE
);

CREATE TABLE transactions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    type ENUM('DEPOSIT', 'WITHDRAW') NOT NULL,
    amount DECIMAL(10,2) NOT NULL CHECK (amount > 0),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

---

## How to Run

### 1. Database Setup

Execute the SQL script above in your MySQL instance.

### 2. Environment Variables

- `DB_URL` — connection string (e.g., `jdbc:mysql://localhost:3306/bank_cli`)
- `DB_USER` — your MySQL username
- `DB_PASSWORD` — your MySQL password

### 3. Execution

Run the `Main.java` class via your preferred IDE or terminal and navigate using the numeric keyboard.
