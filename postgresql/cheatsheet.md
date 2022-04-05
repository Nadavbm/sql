### postgres cheatsheet

#### pgsql commands

terminal connect to db with pgcli \ psql:
``` 
$ pgcli local_database

$ psql postgres://amjith:passw0rd@example.com:5432&sslmode=disable

$ pgcli -h localhost -p 5432 -U amjith app_db
```

in db commands:

`\l` - list databases

`\c` - connect to database

`\dt` - list tables

`\d+ tableName` - describe table

`\du` - list users 

`\df` - list functions

`'df+ functionName` describe function

`\q` - quit

#### basics

##### db

create database - ```CREATE DATABASE dbName;``` 

drop database - ```DROP DATABASE dbName;```

see which db - ```SELECT current_database();```

##### users

see which user connected - ```SELECT current_user();```

##### tables

create table - ```CREATE TABLE tableName (val1  type1, val2 type2);```

create table example:
``` SQL
CREATE TABLE tableName (
    code        char(5) CONSTRAINT firstkey PRIMARY KEY,
    title       varchar(40) NOT NULL,
    id          integer NOT NULL,
    date        date
);
```

drop table - ```DROP TABLE tableName;```

list table records and limit the amount of rows (10):

``` SQL
SELECT  * FROM tableName;

SELECT * FROM tableName limit 10;
```

###### insert queries

create the table to insert data to:
``` SQL
CREATE TABLE people (
    id  SERIAL     PRIMARY KEY,
    username    TEXT    NOT NULL,
    age     INT     NOT NULL,
    title   CHAR(50),
    uaddress CHAR(50)
);
```

insert a record into table `people` (we don't need to add id since it is auto increment - `SERIAL`):

``` SQL
INSERT INTO people (username, age, title, uaddress) VALUES ('nadav', 36, 'devops', 'nakatomi plaza | john mclain 12');

INSERT INTO people (username, age, title, uaddress) VALUES ('shimrit', 42, 'mashachnaasa', 'beersheva'), ('yoram', 72, 'gizbar', 'sde-nehemia'), ('ishtvan', 40, 'midfield', 'beitar');
```

##### select queries

using where, and, in, not, between, is not null, like, or - syntax ```SELECT col FROM tableName WHERE <condition>```

``` SQL
SELECT * FROM people WHERE age > 40;

SELECT username, age FROM people WHERE age > 40 AND title LIKE '%mas%';

SELECT username, title FROM people WHERE age NOT IN (40,72);

SELECT username, age FROM people WHERE age = 40 OR title IS NOT NULL;
```

##### update, insert records

update syntax is ```UPDATE tableName SET col1 = val1, col2 = val2 WHERE <condition>;```

delete sytax is ```DELETE FROM tableName WHERE <condition>```

``` SQL
UPDATE people SET age = 37 WHERE username = 'nadav';

DELETE FROM people WHERE id = 3;
```

#### relational databases
create the tables with relations:
``` SQL
CREATE TABLE people (pid serial primary key not null, name char(50) not null);
CREATE TABLE cars (cid serial primary key not null, car_type char(50) not null, car_license text not null);
CREATE TABLE rent_time (tid serial primary key not null, pid int references people(pid), cid int references cars(cid), rental_hours char(50));
```
insert some data:
``` SQL
insert into people(name) values ('shimon'), ('nehemia'), ('bezazek');
insert into cars(car_type, car_license) values ('shevrolet','123-123-33'), ('renault', '123-124-44');
insert into rent_time (pid, cid, rental_hours) values (1,1, '8.8.2020 17:00 - 12.8.2020 18:00'), (2,1, '20.7.2020 11:00 - 22.7.2020 17:00');
```

cross join:

``` SQL
SELECT people.name. cars.type FROM people CROSS JOIN cars;
```

use inner join (second query uses alias):
``` SQL
SELECT * FROM people inner join rent_time on people.pid = rent_time.pid;

SELECT * FROM people a inner join rent_time b on a.pid = b.pid;
```

outer join (return null if no matching values) - left, right and full outer joins:
``` SQL
SELECT people.name, cars.car_license FROM people LEFT OUTER JOIN cars ON people.pid = cars.cid;

SELECT people.name, cars.car_license FROM people RIGHT OUTER JOIN cars ON people.pid = cars.cid;

SELECT people.name, cars.car_license FROM people FULL OUTER JOIN cars ON people.pid = cars.cid;
```
