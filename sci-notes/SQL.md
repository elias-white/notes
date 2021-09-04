SQL, which stands for _Structured Query Language_, is a language for interacting with data stored in something called a _[[Relational Database]]_.

 In SQL, keywords are not case-sensitive.
 
 It's also good practice to include a semicolon at the end of your query. This tells SQL where the end of your query is!
 

 
 ### `SELECT`
 
 #### `DISTINCT`
 Often your results will include many duplicate values. If you want to select all the unique values from a column, you can use the `DISTINCT` keyword.
 
 ##### `COUNT`
`COUNT(*)` tells you how many rows are in a table. However, if you want to count the number of _non-missing_ values in a particular column, you can call `COUNT` on just that column.

It's also common to combine `COUNT` with `DISTINCT` to count the number of _distinct_ values in a column.

For example, this query counts the number of distinct birth dates contained in the `people` table:

```
SELECT COUNT(DISTINCT birthdate)
FROM people;
```

### `WHERE`
In SQL, the `WHERE` keyword allows you to filter based on both text and numeric values in a table. There are a few different comparison operators you can use:

-   `=` equal
-   `<>` not equal
-   `<` less than
-   `>` greater than
-   `<=` less than or equal to
-   `>=` greater than or equal to

#### `WHERE AND`
Often, you'll want to select data based on multiple conditions. You can build up your `WHERE` queries by combining multiple conditions with the `AND` keyword.

For example,

```sql
SELECT title
FROM films
WHERE release_year > 1994
AND release_year < 2000;
```

gives you the titles of films released between 1994 and 2000.

#### `WHERE OR`
If you want to select rows based on multiple conditions where _some but not all_ of the conditions need to be met SQL has the `OR` operator.

For example, the following returns all films released in _either_ 1994 or 2000:

```sql
SELECT title
FROM films
WHERE release_year = 1994
OR release_year = 2000;
```

When combining `AND` and `OR`, be sure to enclose the individual clauses in parentheses, like so:

```sql
SELECT title
FROM films
WHERE (release_year = 1994 OR release_year = 1995)
AND (certification = 'PG' OR certification = 'R');
```

Otherwise, due to SQL's precedence rules, you may not get the results you're expecting!

#### `WHERE BETWEEN`

Checking for ranges is very common, so in SQL the `BETWEEN` keyword provides a useful shorthand for filtering values within a specified range. This query is equivalent to the one above:

```sql
SELECT title
FROM films
WHERE release_year
BETWEEN 1994 AND 2000;
```

It's important to remember that `BETWEEN` is _inclusive_, meaning the beginning and end values are included in the results!

#### `WHERE IN`
The `IN` operator allows you to specify multiple values in a `WHERE` clause, making it easier and quicker to specify multiple `OR` conditions! Neat, right?

So, an example would simply be:

```sql
SELECT name
FROM kids
WHERE age IN (2, 4, 6, 8, 10);
```

### `NULL` and `IS NULL`
`NULL` represents a missing or unknown value. You can check for `NULL` values using the expression `IS NULL`. For example, to count the number of missing birth dates in the `people` table:

```
SELECT COUNT(*)
FROM people
WHERE birthdate IS NULL;
```

### `LIKE` and `NOT LIKE`
The `LIKE` operator can be used in a `WHERE` clause to search for a pattern in a column. To accomplish this, you use something called a _wildcard_ as a placeholder for some other values. There are two wildcards you can use with `LIKE`:

The `%` wildcard will match zero, one, or many characters in text. For example, the following query matches companies like `'Data'`, `'DataC'` `'DataCamp'`, `'DataMind'`, and so on:

```sql
SELECT name
FROM companies
WHERE name LIKE 'Data%';
```

The `_` wildcard will match a _single_ character. For example, the following query matches companies like `'DataCamp'`, `'DataComp'`, and so on:

```sql
SELECT name
FROM companies
WHERE name LIKE 'DataC_mp';
```

You can also use the `NOT LIKE` operator to find records that _don't_ match the pattern you specify.

### Aggregate Functions
Often, you will want to perform some calculation on the data in a database. SQL provides a few functions, called _aggregate functions_, to help you out with this.

For example,

```sql
SELECT AVG(budget)
FROM films;
```

gives you the average value from the `budget` column of the `films` table. Similarly, the `MAX` function returns the highest budget:

```sql
SELECT MAX(budget)
FROM films;
```

The `SUM` function returns the result of adding up the numeric values in a column:

```sql
SELECT SUM(budget)
FROM films;
```

#### Aggregate Functions with `WHERE`
Aggregate functions can be combined with the `WHERE` clause to gain further insights from your data.

For example, to get the total budget of movies made in the year 2010 or later:

```sql
SELECT SUM(budget)
FROM films
WHERE release_year >= 2010;
```

Aggregate functions can't be used _inside_ `WHERE` clauses.

### `AS` Aliasing
SQL allows you to do something called _aliasing_. Aliasing simply means you assign a temporary name to something. To alias, you use the `AS` keyword, which you've already seen earlier in this course.

For example, in the above example we could use aliases to make the result clearer:

```sql
SELECT MAX(budget) AS max_budget,
       MAX(duration) AS max_duration
FROM films;
```

### `ORDER BY`

The `ORDER BY` keyword is used to sort results in ascending or descending order according to the values of one or more columns.

By default `ORDER BY` will sort in ascending order. If you want to sort the results in descending order, you can use the `DESC` keyword. For example,

```
SELECT title
FROM films
ORDER BY release_year DESC;
```

`ORDER BY` can also be used to sort on multiple columns. It will sort by the first column specified, then sort by the next, then the next, and so on. For example,

```sql
SELECT birthdate, name
FROM people
ORDER BY birthdate, name;
```

sorts on birth dates first (oldest to newest) and then sorts on the names in alphabetical order.

### `GROUP BY`
 In SQL, `GROUP BY` allows you to group a result by one or more columns, like so:

```sql
SELECT sex, count(*)
FROM employees
GROUP BY sex;
```

This might give, for example:

| sex    | count |
| ------ | ----- |
| male   | 15    |
| female | 19    |
  
 
Commonly, `GROUP BY` is used with _aggregate functions_ like `COUNT()` or `MAX()`. Note that `GROUP BY` always goes after the `FROM` clause!

### HAVING
Aggregate functions can't be used inside `WHERE` clauses. For example, the following query is **invalid**:

```sql
SELECT release_year
FROM films
GROUP BY release_year
WHERE COUNT(title) > 10;
```

This means that if you want to filter based on the result of an aggregate function, you need another way! That's where the `HAVING` clause comes in. For example,

```sql
SELECT release_year
FROM films
GROUP BY release_year
HAVING COUNT(title) > 10;
```

shows only those years in which more than 10 films were released.

### `INNER JOIN`

Inner join only joins values in which the keys match across both tables. 

```sql
SELECT c.code, name, region, e.year, fertility_rate, unemployment_rate
FROM countries AS c
	INNER JOIN populations AS p
		ON c.code = p.country_code
 	INNER JOIN economies as e
		ON c.code = e.code
```

#### `USING`

When joining, if the two fields you are joining with have the same labels you can use the `USING` clause instead of the `ON` clause.

```sql
SELECT left_table.id AS L_id,
	left_table.val AS L_val,
	right_table.val AS R_val,
FROM left_table
INNER JOIN right_table
USING (id);
```
The parenthesis is required.

### `CASE`
`CASE` is what you use when you want to join a table in itself.