---
title: SQLite Memo
date: 2018-09-27 11:13:21
tags: SQLite
---
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL    DEFAULT 50000.00
);

INSERT into COMPANY VALUES (1, 'Paul', 32, 'California', 20000)
INSERT into COMPANY VALUES (2, 'Allen', 25, 'Texas', 15000)
INSERT into COMPANY VALUES (3, 'Teddy', 23, 'Norway', 20000)
INSERT into COMPANY VALUES (4, 'Mark', 25, 'Rich-Mond', 65000);
INSERT into COMPANY VALUES (5, 'David', 27, 'Texas', 20000);
INSERT into COMPANY VALUES (6, 'Kim', 22, 'South-Hall', 45000);
INSERT into COMPANY VALUES (7, 'James', 24, 'Houston', 20000);


CREATE TABLE DEPARTMENT(
   ID INT PRIMARY KEY      NOT NULL,
   DEPT           CHAR(50) NOT NULL,
   EMP_ID         INT      NOT NULL
);

INSERT INTO DEPARTMENT (ID, DEPT, EMP_ID)
VALUES (1, 'IT Billing', 1 );

INSERT INTO DEPARTMENT (ID, DEPT, EMP_ID)
VALUES (2, 'Engineering', 2 );

INSERT INTO DEPARTMENT (ID, DEPT, EMP_ID)
VALUES (3, 'Finance', 7 );

INSERT INTO DEPARTMENT (ID, DEPT, EMP_ID) VALUES (4, 'Engineering', 3 );
INSERT INTO DEPARTMENT (ID, DEPT, EMP_ID) VALUES (5, 'Finance', 4 );
INSERT INTO DEPARTMENT (ID, DEPT, EMP_ID) VALUES (6, 'Engineering', 5 );
INSERT INTO DEPARTMENT (ID, DEPT, EMP_ID) VALUES (7, 'Finance', 6 );


---------


select EMP_ID, NAME, DEPT from COMPANY cross join DEPARTMENT;

select EMP_ID, NAME, DEPT from COMPANY inner join DEPARTMENT on COMPANY.ID = DEPARTMENT.EMP_ID;

select EMP_ID, NAME, DEPT from COMPANY left OUTER join DEPARTMENT on COMPANY.ID = DEPARTMENT.EMP_ID;

select EMP_ID, NAME, DEPT from COMPANY inner join DEPARTMENT on COMPANY.ID = DEPARTMENT.EMP_ID union select EMP_ID, NAME, DEPT from COMPANY left OUTER join DEPARTMENT on COMPANY.ID = DEPARTMENT.EMP_ID;

select EMP_ID, NAME, DEPT from COMPANY inner join DEPARTMENT on COMPANY.ID = DEPARTMENT.EMP_ID union all select EMP_ID, NAME, DEPT from COMPANY left OUTER join DEPARTMENT on COMPANY.ID = DEPARTMENT.EMP_ID;

------------------------
CREATE TABLE AUDIT(
    EMP_ID INT NOT NULL,
    ENTRY_DATE TEXT NOT NULL
);

———触发器——
create trigger audit_log after insert on COMPANY
begin
	insert into AUDIT(EMP_ID, ENTRY_DATE) VALUES (new.ID, datetime('now'));
end

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) VALUES (8, 'Paul', 32, 'California', 20000.00 );

select * from sqlite_master where type='trigger';