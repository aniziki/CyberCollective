Time-based blind SQL injection is a subtype of blind SQL injection where the attacker observers the behavior of the database server and the application after combining legitimate queries with SQL commends that cause time delays.

## Example of time-based blind SQL injection:

As an example, let's assume that the following query is meant to display details of a product from a database.
`SELECT * FROM products WHERE id = product_id`

A malicious hacker may provide the following `product_id` value:
`42; WAITFOR DELAY '0:0:10'`

As a result, the query becomes:
`SELECT * FROM products WHERE id = 42; WAITFOR DELAY '0:0:10'`

If the database server is Microsoft SQL Server and the application is susceptible to time-based blind SQL injections, the attacker will see a 10-second delay in the application.

Now that the attacker knows that time-based blind SQL injections are possible, they can provide the following `product_id`:

```
42; IF(EXISTS(SELECT TOP 1 *
  FROM sysobjects
  WHERE id=(SELECT TOP 1 id
    FROM (SELECT TOP 1 id 
      FROM sysobjects 
      ORDER BY id) 
    AS subq
    ORDER BY id DESC)
  AND ascii(lower(substring(name, 1, 1))) = 'a'))
  WAITFOR DELAY '0:0:10'
```

If the name of the first table in the database structure begins with the letter `a`, the second part of this query will be true, and the application will react with a 10-second delay. The attacker can use this method repeatedly to discover the name of the first table in the database structure, then try to get more data about the table structure of this table and finally extract data from the table.

Source:
https://www.invicti.com/learn/blind-sql-injection/