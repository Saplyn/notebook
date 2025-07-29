# SQLite

- [Official Website](https://www.sqlite.org/)
- [Not Implemented SQL Features](https://www.sqlite.org/omitted.html)
- [Frequently Asked Questions](https://www.sqlite.org/faq.html)

## Create

```sql
CREATE TABLE user (
    id INTEGER NOT NULL,
    name TEXT,
    CONSTRAINT 'user_pk_id' PRIMARY KEY (id),
    CONSTRAINT 'user_uq_id' UNIQUE (id)
);

CREATE INDEX user_ind_name
ON user (name);
```

## Read

```sql
SELECT * FROM user;

SELECT name
FROM user
WHERE id == 1;

SELECT count(*) AS user_count
FROM user;
```

## Update

```sql
INSERT INTO user (id, name)
VALUES
    (1, 'First'),
    (2, 'Second');

UPDATE user
SET name = 'Saplyn'
WHERE id == 1;

ALTER TABLE user
ADD COLUMN email TEXT;
```

## Delete

```sql
DELETE FROM user
WHERE name = 'Saplyn';

DROP TABLE user;
```
