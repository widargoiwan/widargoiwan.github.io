---
title: "Adv Sql"
date: 2019-09-10T14:38:01+08:00
draft: true
---

This post concludes my learning of SQL. In the first [post]({{< relref "begin-sql.md" >}}), we covered a lot of ground:

* The mechanics of the `SELECT` statement.
* Commands for CRUD, being `CREATE`, `UPDATE`, `DELETE`.
* Set operations: `union`, `intersect`, `except`
* Subqueries, i.e., `WHERE` appended with `exists`, `in`, `some/any`, `all`, and `unique`.
* Aggregate functions

We briefly covered *joins* as well, but I will cover it again here because I think it is quite important.

In the intermediate [post]({{< relref "inter-sql.md" >}}), we covered:

* Conditional `CASE ... WHEN ... THEN ...` expressions and the specific case of `NULLIF (x, y)` which expands to `WHEN (x = y) THEN ...`.
* The `COALESCE` modifier, which collapses multiple attributes into a single column.

In this post we will cover the following.

* Views
* Joins
