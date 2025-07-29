# Querying

## WHERE Condition

```sql
-- Comparison
SELECT * FROM user WHERE name != 'Saplyn';
SELECT * FROM user WHERE id > 10;

-- `NULL` Check
SELECT * FROM user WHERE email IS NULL;

-- `OR`, `AND` & `NOT`
SELECT * FROM user WHERE name == 'Saplyn' OR name == 'Cobalt';
SELECT * FROM user WHERE name == 'Saplyn' AND surname == 'Miao';
SELECT * FROM user WHERE NOT name == 'Saplyn';

-- `LIKE` Pattern
SELECT * FROM user WHERE name LIKE 'S%';        -- %: any string
SELECT * FROM user WHERE name LIKE 'S_p_yn';    -- _: any character
SELECT * FROM user WHERE name LIKE 'S[ae]plyn'; -- []: any character in the set

-- `IN` & `NOT IN`
SELECT * FROM user WHERE name IN ('Saplyn', 'Cobalt');

-- `BETWEEN AND`
SELECT * FROM user WHERE id BETWEEN 2 AND 30;
```

## Refining Results

```sql
-- `ORDER BY`
SELECT * FROM user ORDER BY name ASC;
SELECT * FROM user ORDER BY name DESC;

-- `LIMIT`
SELECT * FROM user LIMIT 10;

-- `DISTINCT`
SELECT DISTINCT name FROM user;

-- `GROUP BY`
-- Note: columns not in `GROUP BY` must be in an aggregate function.
SELECT rank, count(*) AS user_count
FROM user
GROUP BY rank;

-- `HAVING`
SELECT rank, count(*) AS user_count
FROM user
GROUP BY rank
HAVING user_count > 1;
```

## Joining Tables

```sql
-- `INNER JOIN` returns rows with matching values in both tables.
SELECT *
FROM user
INNER JOIN address
ON user.id = address.id;

-- `LEFT JOIN` preserves all left table's rows, leaving `NULL` for unmatched rows.
-- SQLite has no `RIGHT JOIN`
SELECT *
FROM user
LEFT JOIN address
ON user.id = address.id;

-- `CROSS JOIN`
-- Cartesian product of two tables.
SELECT *
FROM user
CROSS JOIN address;

-- Self `JOIN`
SELECT u1.*, u2.*
FROM user AS u1
INNER JOIN user AS u2
  ON u1.person_id = u2.person_id;

-- Chaining `JOIN`
SELECT *
FROM user
INNER JOIN address ON person.person_id = address.person_id
INNER JOIN phone   ON person.person_id = phone.person_id;
```

## Subqueries

```sql
-- Basic Subquery
SELECT
    stu_id,
    crs_id,
    grade
FROM grades
WHERE grade > (
    SELECT AVG(g2.grade)
    FROM grades AS g2
    WHERE g2.stu_id = grades.stu_id
);

-- ANY / SOME
-- SQLite supports only IN (…) for “= ANY (…)”
SELECT product_name
FROM products
WHERE product_id IN (
    SELECT product_id
    FROM order_details
    WHERE quantity = 10
);

-- ALL
-- No direct “>= ALL (…)”; emulate with NOT EXISTS
SELECT product_name
FROM products p
WHERE NOT EXISTS (
    SELECT 1
    FROM order_details od
    WHERE od.quantity = 10
      AND NOT (p.product_id >= od.product_id)
);

-- IN & NOT IN
SELECT supplier_name
FROM suppliers
WHERE supplier_id IN (
    SELECT supplier_id
    FROM products
    WHERE price > 50
);

-- EXISTS & NOT EXISTS
SELECT supplier_name
FROM suppliers s
WHERE EXISTS (
    SELECT 1
    FROM products p
    WHERE p.supplier_id = s.supplier_id
      AND p.price < 20
);

-- Relational Division
-- students who selected *all* courses
SELECT s.name, s.id
FROM students AS s
WHERE NOT EXISTS (
    SELECT 1
    FROM courses AS c
    WHERE NOT EXISTS (
        SELECT 1
        FROM enrollments AS e
        WHERE e.crs_id = c.id
          AND e.stu_id = s.id
    )
);
```

## INSERT With SELECT

```sql
-- INSERT INTO … SELECT …
INSERT INTO customers (customer_name, city, country)
SELECT supplier_name, city, country
FROM suppliers;

-- SELECT INTO → CREATE TABLE AS SELECT
CREATE TABLE employees_new AS
SELECT id, name, email
FROM users
WHERE id > 200;
```
