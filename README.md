# SdaJdbcJfx
Dedicated to the JDBC part of the course w/ JavaFX

## Acompaning SQL:

```

-- for the JavaFX demo

create database StudentsDemo;
use StudentsDemo;
create table students (
id int primary key not null auto_increment,
first_name varchar(20),
email varchar(20),
language_chosen varchar(20)  );

insert into students values (null, "yohan", "yo@me.com", "java");
insert into students values (null, "Mindi", "mindi@lithu.com", "Dart"),
(null, "IamThat", "Soham@naad.com", "NodeJs"),(null, "Jackson", "jack@sons.com", "Python"),
(null, "Alita", "Battle@angle.com", "C#"),(null, "BEN", "come@Johnson.com", "java3"),
(null, "Perfect", "really@jj0.com", "java"),(null, "NObody", "nono@me.com", "C#"),
(null, "Mr.Hmm", "hmm@me.com", "NodeJs"),(null, "Jesus", "zeus@god.com", "java2");

select * from students;

-- chosen only 3 feilds to insert
insert into students (first_name, email, language_chosen) values ("ABCD", "abcd@x.com", "ABCD");

select * from students;
-- why below code doesnt work
-- update students set language_chosen = 'C#' where language_chosen = 'C';


-- STORED PROCEDURES
CREATE TABLE `teachers` (
  `first_name` varchar(20) NOT NULL,
 `email` varchar(20) DEFAULT NULL,
 `subject` varchar(20) DEFAULT NULL,
 `points` int DEFAULT NULL,
 PRIMARY KEY (`first_name`)
);

INSERT INTO `teachers` VALUES ('yo','oh@me','Java',1000 ),
('Encap','xyz@me.com','C',200 ),('Query','insert@hey.lt','SQL', 1250 ),
('Fxs','effect@','Java',600 ),('Fireboy','fb@fbdb.com','Firebase', 2500 ),
('Andro','A@droid.com','Android',2700 ),('Gamer','game@play.lt','Unity', 1500 ),
('SeQuel','sql@del.com','SQL',1300 ),('Andrax','ax@.com','Android',2600 );


select * from teachers
where points > (select points from teachers where first_name = 'yo');

-- all same
select min(points) from teachers
where subject = 'SQL';

select points from teachers
where subject = 'SQL'
order by points
limit 1;

select points from teachers
where subject = 'SQL'
order by points desc
limit 1,1;

update teachers set points = points + 100 where subject = 'C';


 DELIMITER //
 CREATE PROCEDURE GetAllTeachers()
   BEGIN
   SELECT *  FROM Teachers;
   END //
 DELIMITER ;

DELIMITER //
CREATE PROCEDURE  GetSubjectsTeacher(IN sub_parameter VARCHAR(20))
BEGIN
select * from teachers where subject = sub_parameter;
END //
DELIMITER ;



`twoParameters`(sub_parameter varchar(20), points_parameter int)
BEGIN
select * from teachers where subject = sub_parameter OR points>points_parameter;
END

`updatePoints`( val_parameter int)
BEGIN
update teachers set points = points + val_parameter where subject = 'C';
END

DELIMITER //
CREATE PROCEDURE   GetMaxPointsSubject(OUT reply_ans varchar(20))
BEGIN
select subject into reply_ans from teachers group by subject
order by sum(points) desc
limit 1;
END //
DELIMITER ;


`getCountofPoints`(IN points_parameter int, OUT reply_parameter int)
BEGIN
select count(*) into reply_parameter from teachers where points < points_parameter;
END


`whoTeachesAlita`()
BEGIN
declare x varchar(10) default null;
select language_chosen into x from students where first_name = 'Alita';
select * from teachers where subject =x;
END

`whoTeachesAlitaUpdatePoints`()
BEGIN
declare x varchar(10) default null;
declare y int default 0;
declare msg varchar(30) default null;

select language_chosen into x from students where first_name = 'Alita';
select points into y from teachers where subject =x;
set y = (y/10);
update teachers set points = points + y where subject = x;

set msg = concat('Value Added: ' ,  y);
select msg as NewPoints; -- eg of console print
SIGNAL SQLSTATE'45000' SET MESSAGE_TEXT = msg ; -- error display eg.
END




```
