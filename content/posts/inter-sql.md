---
title: "Intermediate SQL"
date: 2019-09-09T18:13:54+08:00
draft: true
---

## Conditional expressions

Conditional expressions allow us to use insert if/else logic into our `SELECT` statements. In Postgres (and SQL in general, since this is not a Postgres-specific thing), this is done with the `CASE` modifier, which comes after the `SELECT` clause. Recall the syntax for the `SELECT` clause:

```sql
SELECT (DISTINCT) *
FROM relation
WHERE cond_1
GROUP BY attrb_1
HAVING cond_2
ORDER BY attrb_2
LIMIT num
```

Conditional expressions come after the `SELECT` statement, with the `CASE` modifier:

```sql
SELECT name, CASE
  WHEN cond_1 THEN do_1
  WHEN cond_2 THEN do_2
  WHEN cond_3 THEN do_3
  ELSE do_else
END AS grade
FROM table;
```

Within each `cond_n`, we can use Boolean operators as well. For example, we can write the second line as `WHEN (cond_1a) OR (cond_1b)` to express an OR conditional.

The `do_n` is reflected in the resultant table as a separate attribute (column) called `grade`, which is indicated by the part on `END AS grade`.

The conditional expressions allow us to perform what is known as null analysis, which is just a fancy name for using `null` or some `false` value as the `THEN` value. For instance,

```sql
SELECT CASE
  WHEN cond_1 THEN NULL
  ELSE 'fail'
END AS result
FROM quizzes
```

This is the same as:

```sql
SELECT NULLIF (cond_1) FROM table
```

In this case, `cond_1` would be something like `A = B`.

## Coalescing columns into a single column

The `COALESCE` keyboard can then "collapse" (they should have just chosen this word as the keyword instead) by returning the first non-null value in its argument. `null` will be returned *only if* all of the arguments are `null`. For example,

```
| col_1 | col_2 | col_3 |
| ----- | ----- | ----- |
|   a   | null  | pass  |
|   b   | null  | fail  |
|   c   | null  | null  |
| ----- | ----- | ----- |
```

If we call `SELECT col_1, COALESCE (col_2, col_3) AS col_new FROM table`, then we will get the following table.

```
| col_1 | col_new |
| ----- | ------- |
|   a   | pass    |
|   b   | fail    |
|   c   | null    |
| ----- | ------- |
```
