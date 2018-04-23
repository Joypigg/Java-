

## sql数据库中的DDL语言

样表：students

|      id       |    name    |   sex   |
| :-----------: | :--------: | :-----: |
| (primary key) | (not null) | (check) |

| subject |       |
| :-----: | :---: |
|   sid   | sname |

##### add：

ALTER TABLE students ADD COLUMN tel varchar(20);

ALTER TABLE students ADD(addr varchar(20),class_id int);

##### modify：

ALTER TABLE students MODIFY sex varchar(20);

##### drop：

ALTER TABLE students DROP tel;

ALTER TABLE students DROP COLUMN TEL

##### 主键：

ALTER TABLE students ADD CONSTRAINT PK_SID PRIMARY KEY(id);

##### 外键：

ALTER TABLE students ADD CONSTRAINT FK_SID FOREIGN KEY(id) REFERENCES subject(id);

##### 重命名表名:

ALTER TABLE students RENAME TO new_students;

##### 重命名列名：

MySQL :

ABLE students `CHANGE COLUMN` name  names `VARCHAR(15)`;

//其他数据库

ALTER TABLE students RENAME COLUMN tel TO telephone;



