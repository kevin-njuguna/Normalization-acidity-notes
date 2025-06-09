# ACID Properties in Database

## Transactions in DBMS

In Database Management Systems (DBMS), transactions are fundamental operations that allow us to modify and retrieve data. A transaction can be defined as a group of tasks. A single task is the minimum processing unit which cannot be divided further.

**Example:** Think of a transaction like an ATM withdrawal. When we withdraw money, the transaction includes:
- Checking your balance.
- Deducting the money from your account.
- Adding the money to the bank's record.

---

## ACID Properties

To ensure integrity of a database, transactions must be executed in a way that maintains consistency, correctness, and reliability.

**ACID stands for:**
- **Atomicity**
- **Consistency**
- **Isolation**
- **Durability**

These properties define how a transaction should be processed in a reliable and predictable manner. This ensures that even in cases of failures or concurrent accesses, the database remains consistent.

### 1. Atomicity: “All or Nothing”

This property states that a transaction must be treated as an atomic unit, that is, either all of its operations are executed or none. There must be no state in a database where a transaction is left partially completed. States should be defined either before the execution of the transaction or after the execution/abortion/failure of the transaction.

Takeaway:

- A transaction is treated as a single unit.
- All operations must be executed, or none.
- No partial completion allowed.

### 2. Consistency: Maintaining Valid Data States

Consistency ensures that a database remains in a valid state before and after a transaction. It guarantees that any transaction will take the database from one consistent state to another, maintaining the rules and constraints defined for the data. In simple terms, a transaction should only take the database from one valid state to another. If a transaction violates any database rules or constraints, it should be rejected, ensuring that only consistent data exists after the transaction.

Takeaway:

- Transactions take the database from one valid state to another.
- Rules and constraints must be maintained.
- Violating transactions are rejected.

### 3. Isolation: Preventing Interference

 Ensures that concurrent transactions do not interfere. Therefore, multiple transactions can occur concurrently without leading to the inconsistency of the database state. Transactions occur independently without interference. Changes occurring in a particular transaction will not be visible to any other transaction until that particular change in that transaction is written to memory or has been committed.

This property ensures that when multiple transactions run at the same time, the result will be the same as if they were run one after another in a specific order. This property prevents issues such as dirty reads (reading uncommitted data), non-repeatable reads (data changing between two reads in a transaction), and phantom reads (new rows appearing in a result set after the transaction starts).

Takeaway:

- Transactions run independently, even when concurrent.
- No changes are visible until committed.
- Prevents:
  - Dirty reads
  - Non-repeatable reads
  - Phantom reads

### 4. Durability: Persisting Changes

The database should be durable enough to hold all its latest updates even if the system fails or restarts. If a transaction updates a chunk of data in a database and commits, then the database will hold the modified data. If a transaction commits but the system fails before the data could be written on to the disk, then that data will be updated once the system springs back into action.

- Once committed, changes are permanent.
- Survives system failures and restarts.

---

## Database Normalization


Database normalization is a database design principle for organizing data in an organized and consistent way.

It helps to:

- Avoid redundancy
- Maintain integrity
- Eliminate anomalies such as insertion, deletion and updating

Normal forms - these are standard methods to quantify how efficient a database is. There are algorithms that convert a given database into normal forms.

### Why Normalize?

To prevent:
- **Insertion anomalies**
- **Deletion anomalies**
- **Updation anomalies**

Normalization consists of procedures that build efficient database structures.

---

## What is 1NF, 2NF, and 3NF?

Normalization is done in stages:

### First Normal Form (1NF)

**Criteria:**
- Each cell must hold a single value.
- A primary key must exist.
- No duplicate rows or columns.
- Each column must contain atomic values.

**Non-1NF Example:**

| OrderID | CustomerName | Product1 | Quantity1 | Product2 | Quantity2 |
|--------|---------------|----------|-----------|----------|-----------|
| 1      | Alice         | Laptop   | 1         | Mouse    | 1         |
| 2      | Bob           | Keyboard | 1         |          |           |

**Problem:** Repeating groups (Product1, Product2, ...). ). If Alice buys a third product, we'd need Product3, Quantity3, etc., which is not scalable.

**Solution (1NF):**
- Separate the repeating groups into a new table

**Orders Table:**

| OrderID | CustomerName |
|---------|--------------|
| 1       | Alice        |
| 2       | Bob          |

**OrderItems Table:**

| OrderItemID | OrderID | Product  | Quantity |
|-------------|---------|----------|----------|
| 101         | 1       | Laptop   | 1        |
| 102         | 1       | Mouse    | 1        |
| 103         | 2       | Keyboard | 1        |

---

### Second Normal Form (2NF)

The 1NF only eliminates repeating groups, not redundancy. That’s why there is 2NF.

A table is said to be in 2NF if it meets the following criteria:

- Must be in 1NF.
- No partial dependency (non-key attributes must depend on the whole primary key).

**Example:**

| OrderID | Product   | Quantity | ProductPrice |
|---------|-----------|----------|---------------|
| 1       | Laptop    | 1        | 1200          |
| 1       | Mouse     | 1        | 25            |
| 2       | Keyboard  | 1        | 75            |

**Problem:** `ProductPrice` depends only on `Product`, not the full composite key (`OrderID`, `Product`).

**Solution (2NF):**

Remove attributes that depend on only a part of a composite key and place them in a new table.

**OrderItems Table:**

| OrderID | ProductID | Quantity |
|---------|-----------|----------|
| 1       | 101       | 1        |
| 1       | 102       | 1        |
| 2       | 103       | 1        |

**Products Table:**

| ProductID | ProductName | ProductPrice |
|-----------|-------------|--------------|
| 101       | Laptop      | 1200         |
| 102       | Mouse       | 25           |
| 103       | Keyboard    | 75           |

---

### Third Normal Form (3NF)

When a table is in 2NF, it eliminates repeating groups and redundancy, but it does not eliminate transitive partial dependency.


This means a non-prime attribute (an attribute that is not part of the candidate’s key) is dependent on another non-prime attribute. This is what the third normal form (3NF) eliminates.


**Criteria:**
- Must be in 2NF.
- No transitive dependency (non-key attributes depending on other non-key attributes).

**Example:**

| OrderID | CustomerName | CustomerCity | CustomerZipCode |
|---------|--------------|--------------|------------------|
| 1       | Alice        | New York     | 10001            |
| 2       | Bob          | London       | SW1A 0AA         |

**Problem:** Transitive dependency:
`OrderID -> CustomerName -> CustomerCity -> CustomerZipCode`

**Solution (3NF):**

Remove attributes that are transitively dependent on the primary key and place them in a new table.


**Orders Table:**

| OrderID | CustomerID |
|---------|------------|
| 1       | CUST001    |
| 2       | CUST002    |

**Customers Table:**

| CustomerID | CustomerName | CustomerCity | CustomerZipCode |
|------------|---------------|--------------|------------------|
| CUST001    | Alice         | New York     | 10001            |
| CUST002    | Bob           | London       | SW1A 0AA         |
