# Database Scanning Techniques

This document explains common database scanning techniques with examples using a `students` table.

---

## Example Table: `students`

| student_id | name     | age | grade | city      |
|------------|----------|-----|-------|-----------|
| 1          | Alice    | 20  | A     | New York  |
| 2          | Bob      | 21  | B     | Chicago   |
| 3          | Charlie  | 22  | A     | Boston    |
| ...        | ...      | ... | ...   | ...       |

Indexes help speed up data retrieval. Here are two common types:

### Simple Index
A simple index is created on a single column. It helps the database quickly find rows based on that column.

```sql
CREATE INDEX grade_index ON students (grade);
```
- This index allows fast lookups for queries filtering by `grade`.

### Covering Index (Index with Included Columns)
A covering index includes additional columns in the index structure, allowing the database to answer some queries using only the index (without accessing the table).

```sql
CREATE INDEX grade_index ON students (grade) INCLUDE (student_id);
```
- This index not only speeds up searches by `grade`, but also allows queries that need both `grade` and `student_id` to be satisfied directly from the index, improving performance for those queries.

---

## 1. Index Scan

- Uses an index to find matching rows efficiently.
- Good for queries with conditions on indexed columns.

**Example Query:**
```sql
SELECT * FROM students WHERE grade = 'A';
```
- The database uses the index on `grade` to quickly locate all students with grade 'A'.

---

## 2. Index Only Scan

- Retrieves all required data directly from the index (no need to access the table).
- Works when all selected columns are present in the index.

**Example Query:**
```sql
SELECT grade FROM students WHERE grade = 'A';
```
- Since only the `grade` column is needed and it's indexed, the database can answer the query using only the index.

---

## 3. Bitmap Index Scan

- Combines multiple indexes using bitmaps for complex queries.
- Efficient for queries with multiple conditions on indexed columns.

**Example Query:**
```sql
SELECT * FROM students WHERE grade = 'A' AND city = 'Boston';
```
- The database creates bitmaps for `grade` and `city` indexes, combines them, and fetches matching rows.

---

## 4. Parallel Sequence Scan

- The table is divided into chunks and scanned in parallel by multiple processes.
- Improves performance for large tables.

**Example Query:**
```sql
SELECT * FROM students WHERE age > 18;
```
- The database may use parallel workers to scan different parts of the `students` table simultaneously.

---

## 5. Sequence Scan (Full Table Scan)

- Scans every row in the table.
- Used when no suitable index exists or for queries that need most rows.

**Example Query:**
```sql
SELECT * FROM students;
```
- The database reads all rows from the `students` table.

---

**Summary Table**

| Scan Type             | When Used                                  | Example Query                                 |
|-----------------------|--------------------------------------------|-----------------------------------------------|
| Index Scan            | Condition on indexed column                | `SELECT * FROM students WHERE grade = 'A';`   |
| Index Only Scan       | All data in index                          | `SELECT grade FROM students WHERE grade = 'A';`|
| Bitmap Index Scan     | Multiple indexed conditions                | `SELECT * FROM students WHERE grade = 'A' AND city = 'Boston';` |
| Parallel Sequence Scan| Large table, parallel processing           | `SELECT * FROM students WHERE age > 18;`      |
| Sequence Scan         | No index or needs most/all rows            | `SELECT * FROM students;`                     |