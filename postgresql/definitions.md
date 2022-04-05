### definitions

#### schema

schema is a named collection of tables (can container views, indexes, sequences, data types, operators and functions). schema is used in the operating app level and cannot be nested.

to create schema - ```CREATE SCHEMA schemaName;```

tableo inside a schema - ```CREATE TABLE schemaName.tableName ();```

drop schema - ```DROP SCHEMA schemaName;```


#### relational databases

relational datavase is a db that stores and provides access to data that is related to another one. In other words, the database contain tables which some of the columns are related to other columns in a different table within the same db.

create three tables:

``` SQL
CREATE TABLE people (pid serial primary key not null, name char(50) not null);
CREATE TABLE cars (cid serial primary key not null, car_type char(50) not null, car_license text not null);
CREATE TABLE rent_time (rid serial primary key not null, pid int references people(pid), cid int references cars(cid), rental_hours char(50));
```

insert some data:
``` SQL
insert into people(name) values ('shimon'), ('nehemia'), ('bezazek');
insert into cars(car_type, car_license) values ('shevrolet','123-123-33'), ('renault', '123-124-44');
insert into rent_time (pid, rid, rental_hours) value (1,1, '8.8.2020 17:00 - 12.8.2020 18:00'), (2,1, '20.7.2020 11:00 - 22.7.2020 17:00);
```

inner join (second query uses alias):
``` SQL
SELECT * FROM people inner join rent_time on people.pid = rent_time.pid;

SELECT * FROM people a inner join rent_time b on a.pid = b.pid;
```