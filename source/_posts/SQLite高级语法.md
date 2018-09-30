---
title: SQLite高级语法
date: 2018-09-25 14:19:05
tags: SQLite
---
#### 约束
* NOT NULL 		确保某列不能有NULL值
* DEFAULT		当某列没有指定值时，为该列提供默认值
* UNIQUE		某列中所有值是不同的
* PRIMARY KEY 	唯一标志数据库各行
* CHECK			确保某列所有值满足一定条件

#### JOIN
JOIN子句用于结合两个或多个数据库中表的记录。JOIN是一种通过共同值来结合两个表中字段的手段，SQLIte定义了三种类型的连接：
* 交叉连接 CROSS JOIN
  交叉连接（CROSS JOIN）把第一个表的每一行与第二个表的每一行进行匹配。如果两个输入表分别有 x 和 y 行，则结果表有 x*y 行。由于交叉连接（CROSS JOIN）有可能产生非常大的表，使用时必须谨慎，只在适当的时候使用它们。
  `select EMP_ID, NAME, DEPT from COMPANY cross join DEPARTMENT;`
* 内连接 INNER JOIN
  内连接（INNER JOIN）根据连接谓词结合两个表（table1 和 table2）的列值来创建一个新的结果表。查询会把 table1 中的每一行与 table2 中的每一行进行比较，找到所有满足连接谓词的行的匹配对。当满足连接谓词时，A 和 B 行的每个匹配对的列值会合并成一个结果行。
  内连接（INNER JOIN）是最常见的连接类型，是默认的连接类型。INNER 关键字是可选的。
  `select EMP_ID, NAME, DEPT from COMPANY inner join DEPARTMENT on COMPANY.ID = DEPARTMENT.EMP_ID;`
* 外连接 OUTER JOIN
  外连接（OUTER JOIN）是内连接（INNER JOIN）的扩展。虽然 SQL 标准定义了三种类型的外连接：LEFT、RIGHT、FULL，但 SQLite 只支持 左外连接（LEFT OUTER JOIN）。
  外连接（OUTER JOIN）声明条件的方法与内连接（INNER JOIN）是相同的，使用 ON、USING 或 NATURAL 关键字来表达。最初的结果表以相同的方式进行计算。一旦主连接计算完成，外连接（OUTER JOIN）将从一个或两个表中任何未连接的行合并进来，外连接的列使用 NULL 值，将它们附加到结果表中。
  `select EMP_ID, NAME, DEPT from COMPANY left OUTER join DEPARTMENT on COMPANY.ID = DEPARTMENT.EMP_ID;`


#### UNION
* UNION 子句/运算符用于合并两个或多个 SELECT 语句的结果，不返回任何重复的行。为了使用 UNION，每个SELECT被选择的列数必须是相同的，相同数目的列表达式，相同的数据类型，并确保它们有相同的顺序，但它们不必具有相同的长度。
```
select EMP_ID, NAME, DEPT from COMPANY inner join DEPARTMENT on COMPANY.ID = DEPARTMENT.EMP_ID 
union 
select EMP_ID, NAME, DEPT from COMPANY left OUTER join DEPARTMENT on COMPANY.ID = DEPARTMENT.EMP_ID;
```
* UNION ALL 运算符用于结合两个 SELECT 语句的结果，包括重复行
```
select EMP_ID, NAME, DEPT from COMPANY inner join DEPARTMENT on COMPANY.ID = DEPARTMENT.EMP_ID 
union all 
select EMP_ID, NAME, DEPT from COMPANY left OUTER join DEPARTMENT on COMPANY.ID = DEPARTMENT.EMP_ID;
```

#### NULL值
NULL 是用来表示一个缺失值的项。表中的一个 NULL 值是在字段中显示为空白的一个值。带有 NULL 值的字段是一个不带有值的字段。NULL 值与零值或包含空格的字段是不同的


#### 触发器
* 可以指定在特定的数据库表发生 DELETE、INSERT 或 UPDATE 时触发，或在一个或多个指定表的列发生更新时触发。
* SQLite 只支持 FOR EACH ROW 触发器（Trigger），没有 FOR EACH STATEMENT 触发器（Trigger）。因此，明确指定 FOR EACH ROW 是可选的。
* WHEN 子句和触发器（Trigger）动作可能访问使用表单 NEW.column-name 和 OLD.column-name 的引用插入、删除或更新的行元素，其中 column-name 是从与触发器关联的表的列的名称。
* 如果提供 WHEN 子句，则只针对 WHEN 子句为真的指定行执行 SQL 语句。如果没有提供 WHEN 子句，则针对所有行执行 SQL 语句。
* BEFORE 或 AFTER 关键字决定何时执行触发器动作，决定是在关联行的插入、修改或删除之前或者之后执行触发器动作。
* 当触发器相关联的表删除时，自动删除触发器（Trigger）。
* 要修改的表必须存在于同一数据库中，作为触发器被附加的表或视图，且必须只使用 tablename，而不是 database.tablename。
* 一个特殊的 SQL 函数 RAISE() 可用于触发器程序内抛出异常。
```
create trigger audit_log after insert on COMPANY
begin
	insert into AUDIT(EMP_ID, ENTRY_DATE) VALUES (new.ID, datetime('now'));
end
```