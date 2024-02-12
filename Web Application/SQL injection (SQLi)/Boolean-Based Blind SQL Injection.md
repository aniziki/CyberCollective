Boolean-based blind SQL injection is a subtype of blind SQL injection where the attacker observers the behavior of the database server and the application after combining legitimate queries with malicious data using boolean operators.

## Example of boolean-based blind SQL injection

As an example, let's assume that the following query is meant to display details of a product from a database.
`SELECT * FROM products WHERE id = product_id`

At first, a malicious hacker uses the application in a legitimate way to discover at least one existing product ID - in this example, it's product `42`. Then, they can provide the following two values for `product_id`:
`42 and 1=1`
`42 and 1=0`

If this query is executed in the application using simple string concatenation, the query becomes respectively:
`SELECT * FROM products WHERE id = 42 and 1=1`
`SELECT * FROM products WHERE id = 42 and 1=0`

If the application behaves differently in each case, it is susceptible to boolean-based blind SQL injection.

If the database server is a Microsoft SQL Server, the attacker can now supply the following value for the `product_id`:

```42 AND (SELECT TOP 1 substring(name, 1, 1)
  FROM sysobjects
  WHERE id=(SELECT TOP 1 id
    FROM (SELECT TOP 1 id
      FROM sysobjects
      ORDER BY id)
    AS subq
    ORDER BY id DESC)) = 'a'
```

As a result, the sub-query in parentheses after `42 AND` checks whether the name of the first table in the database starts with the letter `a`. If true, the application will behave the same as for the payload `42 AND 1=1`. If false, the application will behave the same as for the payload `42 AND 1=0`.

The attacker can iterate through all the letters and then go on the second letter, third letter, etc. As a result, the attacker can discover the full name of the first table in the database structure. They can then try to get more data about the structure of this table and finally - extract data from the table. While this example is specific to MS SQL, similar techniques exist for other database types.


Source:
https://www.invicti.com/learn/blind-sql-injection/