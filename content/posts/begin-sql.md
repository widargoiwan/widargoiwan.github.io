---
title: "Beginner SQL"
date: 2019-09-03T11:10:11+08:00
draft: true
---
The Structured Query Language (SQL) or Sequel is a declarative programming language that is not Turing-complete. SQL is made up of the following components:

* Data definition language (DDL) that specifies information about relations.
* Data manipulation language (DML) that provides CRUD capabilities.

The DDL provides the following features, which are sometimes listed under SQL as separate components as well.

* Specifying integrity constraints.
* Defining views of the data.
* Specifying the start and end of transactions.
* Specifying access rights to relations and views.

[TOC]



## What is the data definition language (DDL)?

The data definition language specifies information about relations. Such information include:

- Schema for each relation.
- Type of values associated with each attribute, i.e., the domain.
- Integrity constraints on the data.
- Set of indices to be maintained for each relation.
- Security and authorisation information for each relation.
- Physical storage structure of each relation on the disc.

### Types in SQL

There are a few basic types in SQL. In database jargon, these types are known as the domains of the attributes.

* `char(n)` is a fixed-length character string, with user-specified length $n$.
* `varchar(n)` is a variable-length character string, with user-specifed length $n$.
* `int`  is an integer, which is a finite subset of the infinite set of integers. The range ultimately depends on the machine implementation, but usually it is $\pm2^{31} - 1$ .
* `smallint` is a small integer.
* `numeric(p, d)` is a fixed-point number, with user-specified precision of $p$ digits and $d$ digits to the right of the decimal point.
* `float(n)` is a floating-point number, with a user-specified precision of *at least* $n$ digits.

### Basic SQL syntax

#### Create relation

An SQL relation is defined using the `create table` command. For instance,

```sql
create table TABLE_NAME
	(value_1	domain_1	not null,
   value_2	domain_2,
   value_3	domain_3,
   ... etc.
  primary key (X),
  foreign key (Y) references TABLE_Y
);

```

#### Insert tuple

```SQL
insert into TABLE_NAME values (X, Y, Z);
```

#### Delete tuple(s)

Tuples that match a condition can be deleted:

```SQL
delete from TABLE_NAME where CONDITION
```

Or we can delete all the tuples.

```SQL
delete from TABLE_NAME
```

#### Drop table

Dropping a table means that the relation is deleted entirely.

```SQL
drop table TABLE_NAME
```

#### Alter

We can add, remove, and modify attributes (columns) and table/column constraints. An example is provided below:

```sql
ALTER TABLE table_name
ADD attribute domain;

ALTER TABLE table_name
DROP COLUMN attribute;

-- Alter/modify based on different SQL engine
ALTER TABLE table_name
ALTER/MODIFY attribute domain;
```

## `select`

A typical SQL query has the form:

```SQL
select ATTRIBUTE_1,
	ATTRIBUTE_2,
	ATTRIBUTE_3
from RELATION_1,
	RELATION_2,
	RELATION_3
where CONDITION
```

The result of an SQL query is a relation. The `select` clause lists the attributes desired in the result of a query. It corresponds to the projection operation, $\sigma_pc$ of relational algebra. The names in SQL are also case-insensitive.

SQL allows duplicates in relations and query results. To force the elimination of duplicates, we have to use the keyword `distinct` after `select`.

```SQL
select distinct {ATTRIBUTES} from {RELATIONS} where CONDITION
```

An asterisk in the `select` clause denotes all attributes.

```sql
select * from {RELATIONS} where CONDITION
```

An attribute can also be a literal with no `from` clause. In this case, the result is a table with one column and a single row with the value '123'.

```sql
select '123'
```

We can give a column name with `as`. The `as` clause is also used to rename column names.

```sql
select '123' as COLUMN_NAME
```

An attribute can also be a literal combined with the `from` clause.

```sql
select '123' from TABLE_NAME
```

In which case, there will be $n$ number of rows with the value `123`, with $n$ corresponding to the number of tuples in the relation. The `select` clause may contain arithmetic expressions that involve operations.

#### `where`

The `where` clause specifies conditions that the result must satisfy. This corresponds to the selection predicate. SQL allows the use of logical operators `and`, `or` and `not`. The operands of the logical connectives may be expressions that involve the comparison operators (e.g., less than, less than or equal to, etc).

Comparisons can be applied to the results of arithmetic expressions. Arithmetic expressions take precedence over these logical connectives.

#### `from`

The `from` clause lists the relations involved in the query. The `from` clause corresponds to the Cartesian product of the relational algebra. For example, in the following code:

```sql
select * from X, Y
```

The `from` clause generates every possible X-Y relation pair, with attributes from *both* relations. For common attributes, the attributes in the resulting table are renamed using the relation name, e.g., if there is `ID` in both X and Y, then they will be `X.ID` and `Y.ID`.

The Cartesian product is not very useful directly, but it is useful when combined with the `where` clause, which is the selection operator. For example,

```sql
select NAME from INSTRUCTORS, COURSES
where INSTRUCTORS.ID = COURSES.ID
	and INSTRUCTORS.DEPT = 'computer science'
```

#### `rename`

Renaming relations and attributes are done using the `as` clause.

```sql
select distinct P.NAME from T, X as P
where T.ID = P.ID
```

#### `order by`

The clause `order by` sorts the order by which

#### `group by`

Given a transaction:

```sql
SELECT r_name, min(price), max(price)
FROM Sells
GROUP BY r_name
```

Two tuples $t$ and $t'$ belong to the same group if the following expression evaluates to true:

```
t.a1 IS NOT DISTINCT FROM t'.a1 AND ... AND t.aN IS NOT DISTINCT FROM t'.an
```

In less convoluted language, it basically means that the `group by`  operator "collapses" the tuples into a group based on a given attribute. *Only* aggregate functions can be applied to the other attributes within this group. If an aggregate function appears in the `select` clause and there is no `group by` operator, then the `select` clause must not contain any column that is not in an aggregated expression.

#### `having`

The `having` operator can be used under the following 3 situations only:

1. A appears in the `group by` (see above) clause, or
2. A appears in an aggregated expression in the `having` clause, or
3. The primary or candidate key of R appears in the `group by` clause.

### String Operations

SQL includes a string-matching operator for comparisons on character strings. The operator `like` uses patterns that are described using two special characters:

* `%` matches any substring.
* `_` matches any character.

The patterns are *case-sensitive*. There are also a variety of string operations, such as:

* Concatenation using `||`.
* Converting from upper to lower case.

There are certain string operations which are platform-specific. Thus, one should refer to the documentation.

### Set Operations

SQL supports basic set operations like union ($\cup$), intersect ($\cap$), and except ($-$). `union`, `intersect` and `except` eliminate duplicate records. Appending an `all` keyword to these commands preserves duplicate records. All set operations are subject to the same two conditions of union compatibility as in relational algebra:

1. There are the same number of attributes, and
2. The corresponding attributes have the same domains.

In other words, the same columns must be present.

#### `union`

The `union` operator combines the results of two queries into a single table of all matching rows.

#### `intersect`

The `intersect` operator takes the results of two queries and returns only rows that appear in both result sets.

#### `except`

The `except` operator takes the distinct rows of one query and returns the rows that do not appear in a second result set. The `except all` operator does not remove duplicates. The `except` operator does not distinguish between `null`s.

### Null Values

It is possible for tuples to have `null` value for some of their attributes. `null` signifies an unknown value, or that a value doesn't exist. The result of *any* arithmetic expression involving a `null` is `null`. The predicate `is null` can be used to check for null valkues. The predicate `is not null` only succeeds if the value on which it is applied is not null (duh).

SQL treats as *unknown* the result of any comparison involving a `null` value, unless the predicate `is null` or `is not null` is used.

Since the predicate in a `where` clause may involve Boolean operations, the definitions of Boolean operations needs to be extended to deal with the value of unknown. That is to say:

#### and

* `and` – ($\text{True} \and \text{Unknown}=\text{Unknown}$
* `and` – ($\text{False} \and \text{Unknown}=\text{False}$
* `and` – ($\text{Unknown} \and \text{Unknown}=\text{Unknown}$

#### or

* `or` – ($\text{True} \or \text{Unknown}=\text{True}$
* `or` – ($\text{False} \or \text{Unknown}=\text{Unknown}$
* `or` – ($\text{Unknown} \or \text{Unknown}=\text{Unknown}$

The result of the `where` clause predicate is treated as false if it evaluates to unknown.

### Aggregate Functions

Aggregate functions operate on the multiset of values of a column of a relation, and return a *value*, instead of a *relation* as would relational functions. The most common ones are:

* average = `avg`
* minimum = `min`
* maximum = `max`
* sum = `sum`
* number of values = `count`

We may combine aggregate functions with the `as` clause to rename the aggregate function results:

```sql
select NAME, avg(SALARY) as avg_salary
from INSTRUCTOR
group by NAME
```

Attributes in the `select` clause outside of aggregate functions must appear in the `group by` list. For example,

```sql
select NAME, ID, avg(SALARY) as avg_salary
from INSTRUCTOR
group by NAME
```

would be incorrect but

```sql
select NAME, ID, avg(SALARY) as avg_salary
from INSTRUCTOR
group by NAME, ID
```

is correct.

#### `having`

The `having` clause is similar to the `where` clause. It is the aggregate analogue to the `where` clause.

```sql
select NAME, avg(SALARY) as avg_salary
from INSTRUCTOR
group by NAME
having avg(salary) > X
```

Note that in predicates in the `having` clause are applied **after the formation of groups**, whereas predicates in the `where` clause are applied **before the formation of groups**.

### Nested Queries and Subqueries

*This segment is considered 'intermediate'.* Readers are welcome to skip it and return later once you have covered the other sections.

A subquery is a `select X from Y where Z`  expression that is nested within another query. SQL provides a mechanism for the nesting of subqueries. The nesting can be done in a basic SQL query: `select X from Y where Z` by:

* `from <subquery>` clause may be replaced by any valid subquery.
* `where` clause may replaced by an expression of the form `B {operation} <subquery> `.
  * `B` is an attribute and `<operation>` to be defined later.
* `select` where $A_i$ (where $i$ may be any number of attributes) may be replaced by a subquery that *generates a single value*.

For example:

```sql
select NAME
from (select NAME, ID from Y)
where (B <operation> <subquery>)
```

Queries with subquery expressions are known as nested queries. A subquery expression is referred to as an inner query that is nested within an outer query. Scoping rules therefore apply.

1. A tuple variable declared in a subquery or query Q can be used only in Q and any subquery nested *within* Q.
2. If a tuple variable is declared both locally in a subquery *Q* and in an outer query, the local declaration applies in Q.

##### Non-correlated nested queries

A nested query with a subquery that references a tuple variable declared in an outer query is called a correlated nested query. For example,

```sql
SELECT DISTINCT c_name
FROM likes as L
WHERE EXISTS (
    SELECT 1
    FROM sells as S
    WHERE (S.name = L.name));
```

#### Usage of subqueries

Non-scalar subquery expressions can be used in different parts of SQL queries:

* `where` with `exists/not exists`, `in`, `some/any`, `all`, and `unique`.
* `from` with a nested `select` clause.
* `having`

Subqueries can be used to carry out database modifications also.

#### `exists` and `not exists`

The `exists` returns true if the result subquery is non-empty. Otherwise, it returns false. The `exists` subquery can be used to check if there is *at least one* tuple in the `select` subquery.

```SQL
-- Also demonstrating the use of the 'as' keyword
SELECT DISTINCT c_name
FROM t_table AS t_1
WHERE EXISTS (
    	SELECT c_name
   		FROM b_table as t_2
    	WHERE t_1.id = t_2.id);
```

The logical opposite of `exists` is `not exists`.

#### `in`

The subquery must return exactly *one column*. The `in` operator will return false if the result of a subquery is empty. Otherwise, it will return the result of the Boolean expression: $((v = v_1) \or (v = v_2) \or \dots \or (v = v_n))$, where $v$ is the result of the expression.

In other words, the `in` operator checks if there exists some attribute exists in the given subquery.

```sql
SELECT * FROM customers
WHERE country IN (
    SELECT country
    FROM trading_countries AS t_country
    WHERE t_country.rating >= 5
);
```

The `in` and `exists` operators are similar, but actually fundamentally different.

* The `exists` operator will tell you whether a query returned any results, i.e., is the condition *not empty*?
* The `in` operator will compare one value to several and check if there is a match, i.e., is there a value in that matches?

##### Another form of `in`

The other form of `in` is as follows.

```sql
SELECT * FROM customers
WHERE country IN ('Germany', 'US', 'Singapore');
```

#### `unique`

This returns false if the result subquery contains at least two distinct tuples such that $t_1 = t_2$ evaluates to true. The `unique` operator is not widely supported.

#### `some`/`any`

The `some` clause can be used as well. The `some` clause is defined as follows: there exists some tuple in a given relation such that the condition is true for it. The subquery must return exactly one column. It returns false if the result of the subquery is empty. Otherwise, it will return the result of the Boolean expression: $((v \texttt{op} v_1) \or (v \texttt{op} v_2) \or \dots \or (v \texttt{op} v_n))$, where $v$ is the result of the expression.

```sql
select name from instructor
where salary > some (select salary from instructor where dept = 'CS')
```

The `all` clause can be used as well, wherein the predicate must be true for a given condition in all of the tuples.

#### `all`

The logical extension of `some` or `any` to everything in the column. In other words, all the tuples must match.

```sql
select name from instructor
where salary > all (select salary from instructor where dept = 'CS')
```

#### `with` clause

The `with` clause provides a way of defining a temporary relation whose definition is available only to the query in whcih the `with` clause occurs.

```sql
with thing as
	(<subquery>)
select attr
from department, max_budget
where department.budget = max_budget.value
```

The values in the brackets following the `with` clause states the attributes that are taken from the subquery.

#### Scalar subqueries

A scalar subquery is a subquery that returns at most one tuple with one column. A scalar subquery will raise a runtime error if the subquery returns more than one result tuple. If the subquery's result is empty, its return value is then `null`.

### A summary of the `select` clause

```sql
SELECT	DISTINCT select-list
FROM		from-list
WHERE		where-condition
GROUP BY	groupby-list
HAVING		having-condition
ORDER BY	orderby-list
LIMIT		limit-specification
```

1. Compute the cross-product of tables in `from-list`.
2. Select the tuples in the cross-product that evaluate to true for the `where-condition`.
3. Partition the selected tuples into groups using the `groupby-list`.
4. Select the groups that evaluate to true for having the `having-condition` condition, which acts using aggregate functions *only*.
5. For each selected group, generate an output tuple by selecting or computing the attributes or expressions that appear in the `select-list`.
6. Remove any duplicate output tuples.
7. Sort the output tuples based on the `orderby-list`.
8. Remove and trim the number of tuples based on the `limit-specification`.

## Data Modification Language

### Deletion

### Insertion

### Update

The basic update syntax is as such.

```sql
update TABLE_NAME
set attr = new_value
where attr_b = value_b
```

#### Conditional updates with `case`

```sql
update TABLE_NAME
set ATTRIBUTE =
	case
		when COND_1 then VALUE
		when COND_2 then VALUE
		else VALUE
	end
```

#### Updates with scalar subqueries

Scalar subqueries may also be used in the `case`.

```sql
update TABLE_NAME
set ATTRIBUTE =
	(select ATTRIBUTE
		from TABLE_NAME
		where <CONDITION>);
```

### Transactions

A transaction consists of one or more update/retrieval operations, i.e., SQL statements. They are an abstraction to represent a logical unit of work. The `begin` command starts a new transaction, and each transaction *must* end with either a `commit` or `rollback`. For example,

```sql
begin;
update TABLE_NAME
set attr = new_value
where attr_b = value_b;

-- Do other things
commit;
```

The reason for transactions is to ensure that the database maintains ACID principles.

1. **Atomicity**: either all the effects of the transactions are reflected in the database, or none are. In database parlance, *atomicity* is a transaction that is an indivisible and irreducible series of database operations such that either all occur or nothing occurs.
2. **Consistency**: the transaction preserves the consistency of the database.
3. **Isolation**: the transaction is isolated from the effects of other concurrent transaction executions.
4. **Durability**: the effects of a committed transaction persists in the database even in the presence of system failure.

#### Atomicity

A guarantee of atomicity prevents updates to the database occurring only partially, which may cause greater problems than rejecting the whole series outright. As a consequence, the transaction cannot be observed to be in progress by another database client.

At one moment, it is observed that nothing has happened, and at the next it the *entire transaction* has occurred in whole.

> **Banking transfers: an analogy**
>
> A good analogy is a banking transfer. For example, if a person transfers $1,000 from Bank A to Bank B, he expects that the entire transfer is made, or not made at all.

#### Consistency

A guarantee of consistency means that any database transaction must change the affected data *only in allowed ways*. In other words, the constraints on the database must be respected by the transaction. Of course, it does not guarantee the correctness of the transaction in all ways the programmer had intended, but merely that any programming errors cannot result in the violation of any defined database constraints.

#### Isolation

Isolation determines how transaction integrity is visible to other users and systems. Typically, it is defined on a database level *how* or *when* the transactional changes are made visible to other users or clients. Concurrency control is used to handle isolation and guarantee related correctness.

> **Banking transfers (again!)**
>
> When a transfer is made from Bank A to Bank B, when is the value of the account in Bank B updated and the changes shown? This is usually after the transaction is completed.

#### Durability

A guarantee of durability means that transactions that have committed will survive permanently. Durability can be achieved by flushing the transaction to non-volatile storage before acknowledging commitment.

### Constraint Checking

By default, constraints are checked immediately at the end of the SQL statement execution. Any violation will cause the statement to be *rollbacked*. However, constraint checking can also be deferred to the end of transaction execution. In this scenario, any violation will cause the transaction to be *aborted*.

The type of constraint checking can be specified as part of constraint declaration, or configured using set constraints.

#### Handling foreign key constraint violations

Deletion or update of a referenced tuple can potentially violate a foreign key constraint. The SQL specification allows us to state what happens to tuples that are in the referencing relation.

```sql
FOREIGN KEY (a, b, c) REFERENCES t(d, e, f) ON DELETE/UPDATE { action }
```

There are 5 actions we can specify:

1. `no action`: rejects the delete/update if it violates the constraint. In some database systems, it is different from `restrict` because `no action` is a deferred check.

   > A deferred check is a deferred transaction, which is a transaction that is uncommitted when the roll forward phase finishes and that has encountered an error that prevents it from being rolled back. Because the transaction cannot be rolled back, it is *deferred*.
   >
   > Generally, a deferred transaction occurs because, while the database was being rolled forward, an I/O error prevented reading a page that was required by the transaction. The database administrator has to take steps to move the transaction out of `deferred` state. However, that is beyond the scope of this document.

2. `restrict`: similar or identical to `no action`. In MySQL, it is equivalent to `no action`.

3. `cascade`: propagates the delete/update transaction to referencing tuples.

4. `set default`: updates foreign keys of referencing tuples to some default value.

5. `set null`: updates foreign keys of referencing tuples to `null` value.

## Views

A view defines a virtual relation that can be used for querying.

```sql
CREATE VIEW course_info ()
```
