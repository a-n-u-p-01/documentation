### 1️⃣ What is Database Indexing

**Database indexing** is a technique used to **improve the speed of data retrieval** from a database table.

An **index** is a separate data structure that stores **column values with pointers to the actual rows**, allowing the database to find data **without scanning the entire table**.

Without an index, the database performs a **Full Table Scan**, which becomes very slow when the table contains millions of records.

---

### 2️⃣ Example Without Index

Suppose we have a `Users` table:

|id|name|email|
|---|---|---|
|1|Anup|[anup@mail.com](mailto:anup@mail.com)|
|2|Ravi|[ravi@mail.com](mailto:ravi@mail.com)|
|3|Neha|[neha@mail.com](mailto:neha@mail.com)|
|4|Amit|[amit@mail.com](mailto:amit@mail.com)|

Query:

```sql
SELECT * FROM Users WHERE email = 'neha@mail.com';
```

If there is **no index**, the database checks every row:

```
Row1 → Row2 → Row3 → Row4
```

This is **O(n)** complexity.

---

### 3️⃣ Example With Index

If we create an index on `email`:

```sql
CREATE INDEX idx_email
ON Users(email);
```

Now the database can locate the row quickly using the index.

Search complexity becomes approximately:

```
O(log n)
```

---

### 4️⃣ Real Life Analogy

Think of a **book**.

Without an index:

- You search every page.
    

With an index page:

- You directly go to the correct page number.
    

Database indexing works the same way.

---

### 5️⃣ How Index Works Internally

Most relational databases such as  
MySQL and  
PostgreSQL

use a **B-Tree (Balanced Tree)** structure.

Example simplified tree:

```
            40
         /      \
      20         60
    /   \      /    \
   10   30    50    70
```

The database navigates the tree to quickly find the correct record.

---

### 6️⃣ Types of Indexes

#### 1️⃣ Primary Index

Automatically created for the **primary key**.

Example:

```sql
CREATE TABLE Users (
 id INT PRIMARY KEY,
 name VARCHAR(100)
);
```

Characteristics:

- Unique
    
- Fast lookup
    
- One per table
    

---

#### 2️⃣ Secondary Index

Index created on **non-primary columns**.

Example:

```sql
CREATE INDEX idx_name
ON Users(name);
```

Used when queries frequently search by that column.

---

#### 3️⃣ Composite Index

Index created on **multiple columns**.

Example:

```sql
CREATE INDEX idx_name_city
ON Users(name, city);
```

Used when queries filter multiple columns.

Example query:

```sql
SELECT * FROM Users
WHERE name = 'Anup' AND city = 'Kolkata';
```

---

### 7️⃣ Advantages of Indexing

- Faster **search queries**
    
- Improves **read performance**
    
- Essential for **large databases**
    
- Helps optimize **JOIN operations**
    

---

### 8️⃣ Disadvantages of Indexing

Indexes are not always good.

Problems include:

- Extra **storage space**
    
- Slower **INSERT**
    
- Slower **UPDATE**
    
- Slower **DELETE**
    

Because the database must **update indexes whenever data changes**.

---

### 9️⃣ When to Use Index

Create indexes on:

- Columns used in **WHERE clause**
    

```sql
SELECT * FROM Orders WHERE user_id = 10;
```

- Columns used in **JOIN**
    
- Columns used in **ORDER BY**
    
- Columns used in **GROUP BY**
    

---

### 🔟 When NOT to Use Index

Avoid indexing:

- Very **small tables**
    
- Columns with **low uniqueness**
    

Example:

```
gender → male/female
```

This does not benefit much from indexing.

---

### 1️⃣1️⃣ Indexing in System Design

In large-scale systems with millions of records:

Without index:

```
Search time → seconds
```

With index:

```
Search time → milliseconds
```

This is why indexing is critical for **scalable backend systems**.

---

### 1️⃣2️⃣ Interview One-Line Answer

**Database indexing is a technique that improves query performance by creating a data structure (commonly a B-Tree) that allows the database to locate rows quickly without scanning the entire table.**

---
