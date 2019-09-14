---
title: "The Entity-Relationship (ER) Model"
date: 2019-09-10T10:38:16+08:00
draft: true
---
<script src="//yihui.name/js/math-code.js"></script>
<!-- Just one possible MathJax CDN below. You may use others. -->
<script async
  src="//mathjax.rstudio.com/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

The task of creating a database application begins with the needs of the target users. For small applications it is feasible for a database designer who understand the applications to decide directly on the relations that are created, their attributes, and their constraints. Such a direct design process is difficult for real-world applications because they are often highly complex.

The database design process must then be split into several phases.

1. The initial phase characterises fully the data needs of prospective database users.
2. The conceptual-design phase provides a detailed overview of the enterprise. The entity-relationship (ER) model is used to represent conceptual design.
3. A fully-developed conceptual schema indicates the functional requirements of the enterprise. In a specification of functional requirements, users describe the kinds of transactions/operations that are performed on the data. The database designer then reviews the conceptual schema to check if it meets functional requirements.
4. The process of moving from an abstract data model to the implementation of the database proceeds in two final design phases.

### Conceptual design phase

The conceptual schema specifies the entities that are represented in the database, the attributes of those entities, and the relationships.

The conceptual-design phase typically results in the creation of an ER diagram that provides a *graphic representation* of the database's schema.

### Logical design phase

In the logical-design phase, the designer maps the high-level conceptual schema onto the implementation data model of the database system that is used. The implementation data model is the relational data model. This step usually consists simply of mapping the conceptual schema defined with the ER model into a relation schema defined by relational algebra.

The resulting system-specific database schema is then implemented using SQL. The physical features of the database are specified.

## Pitfalls

When designing databases, we must ensure that two major pitfalls are avoided.

1. Redundancy: a bad design repeats information. Redundancy can occur in relational schema. The biggest problem with redundant representation of information is that copies of a piece of information may become inconsistent if the information is updated without taking precautions to update all copies of the information.
2. Incompleteness: a bad design may make certain aspects of the enterprise difficult or impossible to model.

## Entity-Relationship Model

The Entity-Relationship or ER model was developed to facilitate database design by allowing specification of an enterprise schema that represents the overall logical structure of a database.

The ER data model consists of three basic concepts:

1. Entity sets
2. Relationship sets
3. Attributes

## Entity sets

An *entity* is a thing or object in the real world that is distinct from other objects. An entity has a set of properties, and the values for some set of properties may uniquely identify an entity. An *entity set* is a set of entities of the same type that share the same properties (or attributes). For example, the set of all people who are instructors at a given university can be defined as the entity set *instructor*.

An entity is represented by a set of *attributes*. Attributes are descriptive properties possessed by every member of an entity set. The designation of an attribute for an entity set expresses that the database stores similar information concerning each entity in the entity set. However, each entity may have its own value for each attribute. Each entity has a *value* for each of its attributes. The domain of the attribute is its type. A database includes a collection of entity sets, each of which contains any number of entities of the same type.

### Weak entity sets

A weak entity set is an entity set that does not have sufficient attributes to form a primary key. An entity set that *has a primary key* is termed a strong entity set. For a weak entity set to be meaningful, it must be associated with another entity set called the identifying or owner entity set.

Every weak entity must be associated with an identifying entity. The weak entity set is thus said to be *existence dependent* on the identifying entity set. The identifying entity set owns the weak entity set that it identifies. The relationship associating the weak entity set with the identifying entity set is known as the identifying relationship.

The identifying relationship is many-to-one from the weak entity set to the identifying entity set, and the participation of the weak entity set in the relationship is *total.*

The *partial key* of a weak entity set is a set of attributes of the eak entity set that uniquely identifies a weak entity for a given owner entity, that is, since a weak entity's existence is dependent on the existence of the owner entity.

#### Identifying weak entity sets

Although a weak entity set does not have a primary key, we need a means of distinguishing among all those entities in the weak entity set that depend on one particular strong entity. The *discriminator* of a weak entity set is a set of attributes that allows this distinction to be made.

This unique combination of attributes allows us to uniquely identify each entry in the weak entity set. The discriminator of the weak entity set is also known as the *partial key* of the entity set.

* The discriminator of a weak entity set is underlined with a dashed rather than solid line.
* The relationship set connecting the weak entity set to the identifying strong entity is depicted by a double diamond.

### Relationship sets

A relationship is an association among several entities. A relationship set is a set of relationships of the same type. Formally, it is a mathematical relation on n >= 2 (possibly non-distinct) entity sets.

$$\{(e_1, e_2, \dots, e_n) | e_1 \in E_1, e_2 \in E_2, \dots, e_n \in E_n\}$$

The association between entity sets is referred to as participation. Thus, the entity sets $E_1, E_2, \dots, E_n$ participate in the relationship set $R$. A relationship instance in an ER schema represents an association between the named entities in the real-world enterprise that is being modelled.

The function that an entity plays in a relationship is called that entity's role. Since entity sets participating in a relationship set are generally distinct, roles are distinct and are not usually specified. They are useful when the meaning of a relationship needs clarification.

A relationship may have attributes called *descriptive attributes*.

### Attributes

For each attribute, there is a set of permitted values called the domain or the value set. It is akin to the *type* of the variable.

### Participation constraints and roles

Participation constraints introduce constraints on the entity set within the relationship set. There are two kinds of participation constraints:

1. Partial participation constraint: when >0 of E participates in R.
2. Total participation constraint: when >=1 of E participates in R.

Roles are used when one entity set appears two or more times in a relationship set. For example, an entity set `Students` may have two tuples playing the role of `freshman` and `sophomore` in a relationship set `Advised By` connected to `Professors`.

### From the ER Model to the Relational Model

There are three rules:

1. An attribute is mapped to one or more attributes with a domain.
2. An entity set is mapped to a relation. The attributes of the entity set are mapped to the attributes of the relation. The keys are mapped to the primary key of the relation, or to unique combinations of attributes if there is more than one key (to create candidate keys).
3. A relationship set is mapped to a relation. The attributes of the relationship set are mapped to the attributes of the relation. The keys are also mapped, much like an entity set.

However, there are certain minor adjustments that have to be made in the relational model, based on the relationship stated in the ER Model. For example, if we simply apply a primary key constraint to a set of attributes, we are not able to model the "at most 1" relationship in the ER Model. To do so requires us to use the `UNIQUE` keyword.

#### A more detailed explanation

It is quite simple to implement an entity set in SQL. We just need to know the attributes, the candidate keys (which cannot be `null`), and the primary key. Then we translate it into a `CREATE` clause:

```sql
CREATE TABLE Professors (
  pid integer,
  name varchar(50),
  dob date,
  PRIMARY KEY (pid));
```

For ER diagrams that do not have constraints, this is quite easy. However, there are two different ways of interpreting an ER diagram that has constraints. For example, consider the following ER diagram:

![Constraints with Entity-Relationship](/constraints-er.png)

We can write the SQL in two ways.

##### Method 1: `RentedBy` as a separate table
```sql
-- Where Profs and Students have already been created
CREATE TABLE RentedBy (
  sid integer,
  pid integer,
  since date,
  PRIMARY KEY (sid),
  FOREIGN KEY (rid) REFERENCES Profs,
  FOREIGN KEY (sid) REFERENCES Students
);
```

##### Method 2: combine `RentedBy` and `Students`

```sql
CREATE TABLE RentedByStudents (
  sid integer PRIMARY KEY,
  name varchar(50),
  dob date,
  rid integer,
  since date,
  FOREIGN KEY rid REFERENCES Rooms
);
```

For a relationship set with total constraints, things can get trickier. For example, given the following ER diagram:

![Total constraints](/total-constraints-er.png)

How do we translate the total constraint relationship (one student can rent only one room)? Using a separate `RentBy` table fails to capture the total participation constraint:

```sql
CREATE TABLE RentBy (
  sid integer,
  rid integer,
  since date,
  PRIMARY KEY (sid),
  FOREIGN KEY (sid) REFERENCES Students,
  FOREIGN KEY (rid) REFERENCES Rooms
);
```

This happens because we cannot enforce the one-to-one relationship between Students and Rooms. For example, we may have 0 entries for many students, and this would allow for that.

To address this problem, we use the combination `RentBy` and `Students` to enforce a one-to-one relationship:

1. There must be every entry for each student.
2. There must only be one room rented by each student.

The implementation is the same as the above example for partial constraint.

#### Roles in SQL

Given an ER diagram as the following:

![Roles](/roles-er.png)

The corresponding SQL code would be the following.

```sql
CREATE TABLE Dorm (
  instalmentID integer,
  lumpsumID integer,
  PRIMARY KEY (instalmentID, lumpsumID),
  FOREIGN KEY (instalmentID) REFERENCES Dorms(id),
  FOREIGN KEY (lumpsumID) REFERENCES Dorms(id)
);
```

This example, on hindsight, doesn't really make sense. But you get the idea: we enforce the role using a candidate key of the roles, which both refer back to the primary key.

#### Weak entity sets in SQL

A weak entity set, we recall, is one that does not have sufficient attributes to form a primary key. It takes a "has-a" relationship with the dominant entity. For example, the relationship between BookChapters and a Book indicates that the book chapters are weak entity sets.

```sql
CREATE TABLE Books (
  isbn char(30) PRIMARY KEY,
  title char(50),
  author char(50)
);

CREATE TABLE BookChapters (
  num char(30),
  title char(50),
  isbn char(30),
  PRIMARY KEY (num, isbn),
  FOREIGN KEY (isbn) REFERENCES Books
    ON DELETE cascade
);
```

## Object-oriented programming using ER diagramming

ER diagrams can be used to illustrate object hierarchies. This can be done by reflecting the subclass-superclass relationship and decomposing entity sets into subclasses. It is a special relationship known as the "ISA hierarchy". This creates constraints:

1. Overlap constraints: can an entity belong to multiple subclasses? This is satisfied if the entity in the superclass can belong to multiple subclasses.
2. Covering constraints: does an entity in a superclass have to belong to some subclass? This is satisfied if all the entities in a superclass has to belong to some subclass.

#### Differentiating between overlapping and covering constraints

Imagine initialising an "object" from the "class" (i.e., relation). Think of each relation, for example, as a JSON table.

1. To identify an **overlap constraint**, ask whether that "instance" can be both X1 and X2 (where X1 and X2 are both subclasses of X)? If the instance can be both X1 and X2, then there is an overlap, i.e., X1 overlaps with X2.
2. To identify a **covering constraint**, ask whether every "instance" of either subclass has to be an instance of the master class.

The following diagram gives an example:

![Entity-Relationship as OOP](/er-oop.png)

This translates to the following SQL.

### Approach #1: one relation per subclass or superclass

```sql
CREATE TABLE Students (
  sid integer PRIMARY KEY,
  name char(30)
);

CREATE TABLE Undergrads (
  sid integer PRIMARY KEY REFERENCES Students
    ON DELETE CASCADE,
  ged numeric
);

CREATE TABLE Grads (
  sid integer PRIMARY KEY REFERENCES Students
    ON DELETE CASCADE,
  degree varchar(50)
);
```

### Approach #2: one relation per subclass ONLY

In this approach, we "collapse" the superclass of `Students` into each of the subclasses. *This is only applicable if the covering cosntraint is satisfied.*

```sql
CREATE TABLE Undergrads (
  sid integer PRIMARY KEY REFERENCES Students
    ON DELETE CASCADE,
  name varchar(30),
  ged numeric
);

CREATE TABLE Grads (
  sid integer PRIMARY KEY REFERENCES Students
    ON DELETE CASCADE,
  name varchar(30),
  degree varchar(50)
);
```

## Aggregation using ER diagrams

There are three fundamental principles in OOP: encapsulation, inheritance, and composition. In some ways, the ER model preceded OOP because composition is known as *aggregation* and can be modelled using SQL as well. For example, if we have a car made up of multiple parts, we can actually model it using ER (or equivalently in OOP).

## Guidelines for ER design

The ER diagram should do its best to:

1. Capture as many of the application's constraints as possible.
2. Not impose any irrelevant constraint that is not required in the application.
