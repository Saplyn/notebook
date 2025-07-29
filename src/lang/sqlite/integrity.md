# Integrity

## Constraints

```admonish warning
Because [SQLite doesn't support `ALTER TABLE ADD CONSTRAINT`](https://stackoverflow.com/a/1884841/21117414),
the only way to specify a constraint is do it when creating the table.
```

```sql
-- `NOT NULL`
CREATE TABLE user (
    id TEXT NOT NULL,
);

-- `DEFAULT`
CREATE TABLE user (
    name TEXT DEFAULT 'Anonymous',
);

-- `PRIMARY KEY`
-- Note that `NOT NULL` is NOT implied for primary keys in SQLite
-- ref: https://sqlite.org/lang_createtable.html#the_primary_key
CREATE TABLE user (
    id TEXT NOT NULL,
    CONSTRAINT 'users_pk_id' PRIMARY KEY (id)
);

-- `FOREIGN KEY`
CREATE TABLE user (
    id TEXT,
    CONSTRAINT 'users_fk_id' FOREIGN KEY (id) REFERENCES other_table(id)
);

-- `UNIQUE`
CREATE TABLE user (
    email TEXT UNIQUE
);

-- `CHECK`
CREATE TABLE user (
    age INTEGER CHECK (age >= 0)
);
```

## Transactions

Transactions are used to ensure that a series of SQL statements are executed as
a single unit. If any of the statements fail, the entire transaction is rolled
back.

```sql
-- `BEGIN TRANSACTION` and `COMMIT`
BEGIN TRANSACTION;
-- SQL statements...
COMMIT;

-- `ROLLBACK` to undo the transaction
BEGIN TRANSACTION;
-- SQL statements...
ROLLBACK;
```
