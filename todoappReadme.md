# Lesson 3: ToDoApp Readme (abdullah)

ToDoApp is an user based app. Each user can do something in his own environment.
In our case of homework, there is no admin.
Users can; - Create todo list - Add todo item(s) to list - Delete todo list - Delete todo item(s) - Mark item(s) as done - Add reminder to list

## here is methods

## What you can GET:

| path                | What to get                      |
| ------------------- | -------------------------------- |
| /username/list      | All Lists                        |
| /username/todo      | All todo items (with list names) |
| /username/done      | All items marked done            |
| /username/reminders | Reminders with list names        |

## What you can PUT (please check request body instructions):

| path  | What to update                    |
| ----- | --------------------------------- |
| /list | Adds reminder(from body)          |
| /todo | Marks item(s) as done (from body) |

## What you can POST (please check request body instructions):

| path  | What to create                    |
| ----- | --------------------------------- |
| /list | Create a list with provided name  |
| /todo | Add todo item(s) to specific list |

## What you can DELETE (please check request body instructions):

| path   | What to delete                  |
| ------ | ------------------------------- |
| /lists | Delete a list with todo items   |
| /todo  | Delete todo item(s) from a list |

## Request Body

Request body is in the same shape in all cases.

```
{
  "user": "abdullah",
  "list": {
    "name": "nodejs",
    "reminder": "2012-10-05 12:05:00"
  },
  "todo": [{ "description": "drink tea" }]
}
```

### Requirements in cases of request (Request body instructions)

`"user"` and `"list":{"name"}` areas are mandatory in all cases

1- POST /list
`"reminder": "2012-10-05 12:05:00"` is optional

`"todo": []` content of todo array is ignored

2- POST /todo
`"reminder": "2012-10-05 12:05:00"` is ignored

`"todo": [{ "description": "todoItemDescription" }]` is mandatory. All todo items in array will be added

3- PUT /list
`"reminder": "2012-10-05 12:05:00"` is mandatory in mysql datetime format

`"todo": []` content of todo array is ignored

4- PUT /todo
`"reminder": "2012-10-05 12:05:00"` is ignored

`"todo": [{ "description": "todoItemDescription" }]` is mandatory. All todo items in array will be marked as done

5- DELETE /list
`"reminder": "2012-10-05 12:05:00"` is ignored

`"todo": []` content of todo array is ignored.

6- DELETE /todo
`"reminder": "2012-10-05 12:05:00"` is ignored
`"todo": [{ "description": "todoItemDescription" }]` is mandatory. All todo items in array will be deleted.

### USERNAMES

defined usernames are:
 zehra 
 yagoup 
 veli 
 ists 
 utku 
 unmesh 
 talip 
 suheyb 
 nadin 
 irfan 
 igor 
 hamza 
 hani 
 ghufran 
 andrej 
 alusine 
 abdullah 

#### 1NF (4 rules)

- Rule 1 : Single valued attributes (each column should have atomic value, no multiple values)
- Rule 2 : Attribute domain should not change
- Rule 3 : Unique names for attributes / columns
- Rule 4 : Order does not matter

#### 2NF

No partial dependency. (i.e. no field should depend on part of the primary key)
Example

```
Score table (student_ID, subject_ID, score, teacher)
Subject table (subject_ID, subject Name)
```

#### 3NF

No transitive dependency (i.e. no field should depend on non-key attributes).

#### Boyce Codd Normal Form (3.5 NF)

for any dependency A → B, A should be a super key.

#### 4NF

No multi-value dependency.

### Complicated values to store in MySQL

    - Storing prices (floating point errors)
    - Storing dates (datetime vs. timestamp)
    - datetime : fixed value (joining date of employee): has a calendar date and a wall clock time
    - timestamp : unix timestamp, seconds elapsed from 1 Jan 1970 00:00 in UTC (takes timezone into consideration)

### Database transactions

- A transaction is a set of commands that you want to treat as "one command." It has to either happen in full or not at all.

- A classical example is transferring money from one bank account to another. To do that you have first to withdraw the amount from the source account, and then deposit it to the destination account. The operation has to succeed in full. If you stop halfway, the money will be lost, and that is Very Bad.

* To start transaction:

```
mysql> start;
OR
mysql> begin transaction;
```

- To commit, use `commit;` and to abort, use `rollback;`
- Note that `autocommit` variable should be set to false for rollback to work.

```
mysql> set autocommit = 0;
```

### ACID properties

- **Atomicity** : states that database modifications must follow an “all or nothing” rule.
  Each transaction is said to be “atomic.”
  If one part of the transaction fails, the entire transaction fails.
- **Consistency** : states that only valid data will be written to the database. If, for some reason, a transaction is executed that violates the database’s consistency rules, the entire transaction will be rolled back, and the database will be restored to a state consistent with those rules.
- **Isolation** : requires that multiple transactions occurring at the same time not impact each other’s execution.
- **Durability** : ensures that any transaction committed to the database will not be lost. Durability is ensured through the use of database backups and transaction logs that facilitate the restoration of committed transactions in spite of any subsequent software or hardware failures.

### SQL injection

Some SQL clients accept input from user to fabricate the queries.
A malicious user can tweak the input so as to acquire more information from the database or
to destroy the database (literally!). Demo program `sql-injection.js` is in the `Week3` folder.

Consider the following query `SELECT name, salary FROM employees where id = X`.

#### Injection to get more information

```
If X is `101 OR 1=1`, then the query returns all records because 1=1 is always true
SELECT name, salary FROM employees where id = 101 OR 1=1;
```

#### Injection to destroy the database

```
If X is `101; DROP database mydb`, then the query will delete the entire database
SELECT name, salary FROM employees where id = 101; DROP database mydb;
```

mysqljs prevents the second injection by not allowing multiple SQL statements
to be executed at once.

### Procedures

- Procedures in SQL (aka Stored procedures) are similar to functions in other programming languages.
  i.e. You can define them once and call them multiple times. However, it should be noted that
  MySQL has two different concepts : functions and procedures.
  This stack overflow post has an excellent answer that describes the
  [difference between MySQL functions vs procedures](https://stackoverflow.com/questions/3744209/mysql-stored-procedure-vs-function-which-would-i-use-when)

- There are two scenarios in which procedures are particularly useful:
  (credits to [this stack overflow post](https://stackoverflow.com/questions/12631845/when-should-i-use-stored-procedures-in-mysql))

1. When we want to entirely encapsulate access to the database by forcing apps to use
   the stored procedures. This can be good for an organization with a strong/large database group
   and a small/weak programming team.
   It's also helpful when you have multiple code bases accessing the database,
   because they all get one interface, rather than each writing their own queries, etc.
2. When you're repetitively doing something that should be done in the database.

- To create a procedure, use the following syntax:

```
Example:
delimiter //
create procedure countCountries (OUT param1 int)
BEGIN
    select count(*) into param1 from country;
END
//

delimiter ;
```

- To see existing procedures, use the following command:

```
mysql> show procedure status where db = 'dbname';
```

- To call the procedure, use the following command:

```
mysql> call countCountries(@result);

mysql> select @result;
```

### Understanding the asynchronous nature of database queries

Jim (@remarcmij) wrote these [excellent demo programs](https://github.com/remarcmij/database_examples)
for better understanding. Do check them out.

## Reference Material

- [Floating Point Inaccuracy](http://stackoverflow.com/questions/2100490/floating-point-inaccuracy-examples#2100502)
- [Example Entity Relationship Diagram (including associative entities)](http://users.csc.calpoly.edu/~jdalbey/308/Lectures/HOWTO-ERD.html)
- Scaffolding tools:
  - [Yeoman](http://yeoman.io) - General framework for creating and scaffolding all types of projects
  - [Sails](http://sails.js) - Lightweight framework for generating APIs and web server apps in Node
  - [Loopback](http://loopback.io/) - A more "enterprise-ready" framework for generating and managing APIs.
