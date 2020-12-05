# Class Database

## Submission

Below you will find a set of tasks for you to complete to set up a databases of students and mentors.

To submit this homework write the correct commands for each question here:

```sql


```

When you have finished all of the questions - open a pull request with your answers to the `Databases-Homework` repository.

## Task

1. Create a new database called `cyf_classes` (hint: use `createdb` in the terminal)
2. Create a new table `mentors`, for each mentor we want to save their name, how many years they lived in Glasgow, their address and their favourite programming language.

~~~
CREATE TABLE mentors(
  id            SERIAL PRIMARY KEY,
  name          VARCHAR(30) NOT NULL,
  glasgow_years INT NOT NULL,
  address       VARCHAR(120) NOT NULL,
  fav_prog_language    VARCHAR(30) NOT NULL
);
~~~

3. Insert 5 mentors in the `mentors` table (you can make up the data, it doesn't need to be accurate ;-)).

~~~
INSERT INTO mentors (name, glasgow_years, address, fav_prog_language) VALUES ('John Smith','5','11 New Road','java');
INSERT INTO mentors (name, glasgow_years, address, fav_prog_language) VALUES ('Shehnaz','12','42 Duck House','Javascript');
INSERT INTO mentors (name, glasgow_years, address, fav_prog_language) VALUES ('Carlos','3','14 Cendra','Ruby');
INSERT INTO mentors (name, glasgow_years, address, fav_prog_language) VALUES ('Benesa schrez','9','02 old road','C++');
INSERT INTO mentors (name, glasgow_years, address, fav_prog_language) VALUES ('Ramon Llul','10','13 New Road','Javascript');
~~~

4. Create a new table `students`, for each student we want to save their name, address and if they have graduated from Code Your Future.

~~~
CREATE TABLE students(
  id            SERIAL PRIMARY KEY,
  name          VARCHAR(30) NOT NULL,
  address       VARCHAR(120) NOT NULL,
  graduated     VARCHAR(30) NOT NULL
);
~~~

5. Insert 10 students in the `students` table.

~~~
INSERT INTO students (name, address, graduated) VALUES ('John Nava','2 New Road','yes'); 
INSERT INTO students (name, address, graduated) VALUES ('Juan Albez','15 Paloma','yes'); 
INSERT INTO students (name, address, graduated) VALUES ('Alexender Flaming','7 Lluna','no'); 
INSERT INTO students (name, address, graduated) VALUES ('Stuard Broad','10 Sant Pau','no'); 
INSERT INTO students (name, address, graduated) VALUES ('Gayle Jordan','11 West Feri','yes'); 
INSERT INTO students (name, address, graduated) VALUES ('Leoarndo Rafsan','21 Hamilton Road','yes'); 
INSERT INTO students (name, address, graduated) VALUES ('Victar Alex','12 Dock Road','yes'); 
INSERT INTO students (name, address, graduated) VALUES ('Nicolas Fernandes','14 Charls Road','yes'); 
INSERT INTO students (name, address, graduated) VALUES ('Franko Albert','27 Dhanmondi','yes'); 
INSERT INTO students (name, address, graduated) VALUES ('Newton Dario','11 Banani','no'); 
~~~

6. Verify that the data you created for mentors and students are correctly stored in their respective tables (hint: use a `select` SQL statement).

~~~
SELECT * FROM students WHERE graduated = FALSE;
~~~

7. Create a new `classes` table to record the following information:

   - A class has a leading mentor
   - A class has a topic (such as Javascript, NodeJS)
   - A class is taught at a specific date and at a specific location

~~~
create table classes (
	id serial primary key,
	mentor serial references mentors(id),
	topic varchar(30),
	date  date not null,
	location  varchar(30) not null
);
~~~


8. Insert a few classes in the `classes` table

~~~
insert into classes (mentor, topic, date, location) values(2, 'javascript', now(), 'online');
insert into classes (mentor, topic, date, location) values(3, 'Ruby', now(), 'online');
insert into classes (mentor, topic, date, location) values(1, 'java', now(), 'online');
~~~

9. We now want to store who among the students attends a specific class. How would you store that? Come up with a solution and insert some data if you model this as a new table.

~~~
create table attendance (
	id serial primary key,
	student serial references students(id),
	class serial references classes(id)
);
insert into attendance (student,class) values(1,2);
insert into attendance (student,class) values(2,1);
insert into attendance (student,class) values(10,3);
insert into attendance (student,class) values(5,1);
insert into attendance (student,class) values(8,2);
~~~

10. Answer the following questions using a `select` SQL statement:
    - Retrieve all the mentors who lived more than 5 years in Glasgow
    - Retrieve all the mentors whose favourite language is Javascript
    - Retrieve all the students who are CYF graduates
    - Retrieve all the classes taught before June this year
    - Retrieve all the students (retrieving student ids only is fine) who attended the Javascript class (or any other class that you have in the `classes` table).

~~~
select * from mentors where glasgow_years > 5;
select * from mentors where fav_prog_language = 'Javascript';
select * from students where graduated = 'yes';
select * from classes where date < '01/06/2020';
select * from attendance where class = 1;
~~~

# Delete tables 
~~~
drop table if exists mentors;
drop table if exists students;
drop table if exists classes;
~~~