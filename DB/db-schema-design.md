# Database design

Course Link: https://www.youtube.com/watch?v=e7Pr1VgPK4w&list=PLlTjty5ceOnd-sCYEHlFO0JRg2liaFvxv&index=1

## Content

- [Database design](#database-design)
  - [Content](#content)
  - [Data integrity](#data-integrity)
    - [entity integrity](#entity-integrity)
    - [referential integrity](#referential-integrity)
    - [Domain integrity](#domain-integrity)
  - [atomic value](#atomic-value)
  - [Relationships](#relationships)
    - [one to one](#one-to-one)
    - [one to many](#one-to-many)
    - [many to many](#many-to-many)
  - [Keys](#keys)
    - [Key Index](#key-index)
    - [Types of keys](#types-of-keys)
    - [Foreign key constraint](#foreign-key-constraint)
  - [Basic ERM(entity relationship model)](#basic-ermentity-relationship-model)
  - [Normalization](#normalization)
    - [1NF](#1nf)
    - [2NF](#2nf)
    - [3NF](#3nf)
  - [Index](#index)
    - [cluster index](#cluster-index)
    - [non-cluster index](#non-cluster-index)
- [Join](#join)
  - [Inner-Join](#inner-join)
  - [Outer-Join](#outer-join)

## Data integrity

### entity integrity

each record should be unique, if there is duplicate record, we should add more attribute to distinguish them.

### referential integrity

problem: table A reference entity from other table B. Remove some entity from Table B but not updating table A, then table A is going to reference non-exist entity.

Use foreign key constraint to solve this problem. If one record is removed, then all related record from other table wil be removed.

### Domain integrity

specific valid data format for field. Use data type.

## atomic value

split one field into multiple field.  
For example:

- name -> firstName, lastName
- address -> street, apartment, city, state, country

## Relationships

### one to one

- husband <-> wife
- person <-> ssn

**DB design:**

```
save in same table: "ID, SSN"
save in separate table: "ID, cardID", "cardID, cardInfo"
```

### one to many

- user -> [comment1,comment2,comment3]
- king -> [wife1, wife2, wife3]

**DB design:**

Give a foreign key in `many` side pointing back to `one` side.

Parent table is on `one` side with primary key,
child table is on `many` side with foreign key pointing back to parent.

For example, one user can have multiple cards, each card is assigned to one user.

```
we include userID as foreign key in card table.
user: "userID"
card: "cardID, userID, cardInfo"
```

### many to many

Create an intermediary table to break M : N relationship into 1:M and 1:N.

**DB design:**

lets say, one student can take many class, one class can hold many students.  
We can represent this M to N relationship with 3 tables:

- class table
- student table
- intermediary class_student table.

| class_id | class_name |
| -------: | ---------- |
|        1 | math       |
|        2 | english    |
|        3 | physics    |

| student_id | student_name |
| ---------: | ------------ |
|          1 | jone         |
|          2 | lucy         |
|          3 | lee          |

| student_id | class_id |
| ---------: | -------- |
|          1 | 1        |
|          1 | 2        |
|          2 | 2        |
|          3 | 1        |
|          3 | 3        |

## Keys

### Key Index

key is a type of index. Index is built with special data structure that improve lookup speed with additional storage. Without index, database need to iterate the whole table to find specific records.

### Types of keys

- **surrogate key** has no semantic meaning, usually it is generated by database system as an sequential id. It is not visible to users. It is useful when sometime you cannot find a natural key.
- **natural key** is key with semantic meaning.
- **Primary key** is one attribute or a combination of attributes that uniquely identifies a record.
- **Secondary key** is a non-unique field
- **Candidate Key** or **Alternate Key** is one attribute or a combination of attributes that can uniquely identify a record but not selected as **primary key**.
- **superkey** can uniquely identify a record, **Candidate keys** are a special subset of **superkey** that do not have any extraneous information in them.
- **compound key** consist only foreign key
- **composite key** consist foreign key or non-foreign key

```
Example for super key:
Imagine a table with the fields <Name>, <Age>, <SSN> and <Phone Extension>.
This table has many possible superkeys:
three of these are <SSN>, <Phone Extension, Name> and <SSN, Name>.
But only <SSN> is a candidate key
```

### Foreign key constraint

FK constraint protect data integrity after updating parent, at the same time the child should get updated too.

specify constraint On delete or update

**type of constraint**:

- Restrict(No action) - throw error
- cascade - perform update or delete to all children
- set null - if update or delete parent, set all related children fk to null

## Basic ERM(entity relationship model)

![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/91/ERD-artist-performs-song.svg/800px-ERD-artist-performs-song.svg.png)

- O - represent FK can be null
- | - represent FK cannot be null

## Normalization

https://en.wikipedia.org/wiki/Database_normalization#Example_of_a_step_by_step_normalization

Database normalization is a process of structuring table in normal form in order to reduce data redundancy and improve data integrity.

The process is progressive, a higher level of normal form cannot be achieved unless the previous level have been satisfied.

### 1NF
value of each column must be atomic. If one column contains a set of value, then it doesn't apply.

### 2NF

every non-key attribute must depend on the every whole candidate key. If attribute only depend on partial of the candidate key, then 2NF doesn't apply.

### 3NF

3NF table doesn't have transitive dependencies. 
Transitive dependency: attribute C depend on B, B depend on primary key. 

## Index

### cluster index

table row stored in the same order as cluster index, therefore, there is only one cluster index.

### non-cluster index
non-cluster index create a list that has pointer pointing back to original table row.

# Join

## Inner-Join

select records that have matching values in both tables.

```sql
SELECT username, title, comment
FROM user
INNER JOIN comment
ON user.user_id = comment.user_id
INNER JOIN video
ON comment.video_id = video.video_id
```

## Outer-Join

shows all record of one table. If there is no matching record, the column is null.

- left join
- right join
- full join