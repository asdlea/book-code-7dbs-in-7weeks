# Homework Postgres

## Day 1

### Find

1. <https://www.postgresql.org/docs/>
2. **CLI**  
    - **\\?**: help on CLI commands (some of them very useful, could avoid writing some queries).
    - **\h**: help on SQL commands.
3. When using CREATE TABLE we can choose the way we REFERENCE multiple columns in other table (FOREIGN KEYS):  
    - **MATCH FULL**: will not allow one column to be null unless all foreign key columns are null; if they're all null, the row is not required to have a match in the referenced table.  
    - **MATCH SIMPLE**: allows any of the foreign key columns to be full; if any of them are null, the row is not required to have a match in the referenced table. Basically: if one column is NULL the constraint is ignored.  

### Do

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

## Day 2

### Find

1. https://www.postgresql.org/docs/current/functions-aggregate.html
2. DBeaver

### Do

1.

```sql
CREATE OR REPLACE RULE rule_delete_venuesAS ON DELETE TO venues
    DO INSTEAD
    UPDATE venues
        SET active = false
    WHERE venue_id = OLD.venue_id;
```

2.

```sql
SELECT * FROM crosstab(
    'SELECT extract(year from starts) as year, extract(month from starts) as month, count(*)
    FROM events
    GROUP BY year, month
    ORDER BY year, month',
    'SELECT generate_series(1,12)'
) AS (
    year int,
    jan int, feb int, mar int, apr int, may int, jun int, jul int, aug int, sep int, oct int, nov int, dec int
) ORDER BY YEAR;
```

3.

```sql
SELECT * FROM crosstab(
   'SELECT
       EXTRACT(WEEK FROM starts) - EXTRACT(WEEK FROM date_trunc(''MONTH'', starts)) + 1 AS week_of_month,
       EXTRACT(DOW FROM starts) AS day_of_week,
       COUNT(*) AS event_count
     FROM
       events
     WHERE
       EXTRACT(MONTH FROM starts) = 2 -- Replace with the desired month
     GROUP BY 1, 2
     ORDER BY 1, 2',
   'SELECT generate_series(0, 6)'
) 
AS (
   week_of_month INTEGER,
   Sunday INTEGER,
   Monday INTEGER,
   Tuesday INTEGER,
   Wednesday INTEGER,
   Thursday INTEGER,
   Friday INTEGER,
   Saturday INTEGER
);
```
