# Homework Postgres

## Find

1. https://www.postgresql.org/docs/
2. **CLI**  
    - **\\?**: help on CLI commands (some of them very useful, could avoid writing some queries).
    - **\h**: help on SQL commands.
3. When using CREATE TABLE we can choose the way we REFERENCE multiple columns in other table (FOREIGN KEYS):  
    - **MATCH FULL**: will not allow one column to be null unless all foreign key columns are null; if they're all null, the row is not required to have a match in the referenced table.  
    - **MATCH SIMPLE**: allows any of the foreign key columns to be full; if any of them are null, the row is not required to have a match in the referenced table. Basically: if one column is NULL the constraint is ignored.  

## Do

1.

```sql
SELECT *
FROM pg_class
WHERE relname IN ('countries', 'cities', 'venues', 'events');
```

2.

```sql
SELECT c.country_name
FROM events e
    JOIN venues v ON e.venue_id=v.venue_id
    JOIN countries c ON c.country_code=v.country_code
WHERE e.title='Fight Club';
```

3.

```sql
ALTER TABLE venues
ADD COLUMN active BOOL DEFAULT TRUE;
```
