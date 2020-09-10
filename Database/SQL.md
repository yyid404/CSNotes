# SQL

​		若想用MySQL中的关键字作为字段，可用``括起，使其成为文本来处理，而不会当成数据库来解析。

## 数据类型

**decimal**：底层以字符串的形式存储，可以保障绝对的精度。存储空间不固定，由精度M决定。

**enum**：只能从给定的备选项中任选其一，enum类型总有一个默认值，若enum列声明为null，则默认值为null。若enum列声明为not null，则默认值为列表的第一个元素。

**set**：能从给定的必选项中选择一个或多个。

## 运算符/关键字

### show

```sql
//查看当前MySQL中的数据库
show databases

//显示数据库的创建
show create database db_name
```

### use

切换数据库

```sql
use db1
```

### desc

简单描述表的结构

```sql
desc tb_name
```

### create

创建数据库

```sql
create database [if not exists] db_name
```

### drop

删除表

```sql
drop database [if exists] db_name
```

### delete

删除记录

```sql
delete from tb_name where 判断条件
```

> DELETE p1就表示从p1表中删除满足WHERE条件的记录。
>
> ```sql
> DELETE p1 FROM Person p1,
>  Person p2
> WHERE
>  p1.Email = p2.Email AND p1.Id > p2.Id
> ```

### truncate

**drop**：删除整个表

**trustcate**：删除全部记录，但不删除表。将当前表重新映射一段存储空间，并把旧空间标记为可用。会将高水线复位。

**delete**：删除部分记录，不影响所用extent，高水线保持原位置不动。

> 执行速度：drop>trustcate>delete

### alter

```sql
//修改字符集
alter db_name character set charset_name

//修改默认值
alter table tableName alter column set default value xx

//删除列field
alter table tb_name drop (field)

//追加列
alter table tb_name add (field1 datatype,add field2 datatype,add field3 datatype,…)
```

### change

可进行所有修改表的操作。修改列名或类型，若不修改其中一项，则写与原值相同的即可。

```sql
alter table tb_name change field1 new_field1 datatype,change field2 new_field2 datatype,…
```

### update

```sql
//更新原有表符合判断条件的行中的指定列
update tb_name set column1=value1,column2=value2,…where 判断条件
```

### modify

只可修改列的类型	

```sql
alter table tb_name modify field1 datatype,modify field2 datatype,…
```

### rename

移动表，若在同一个库中移动，可理解为重命名。

```sql
rename table old_db_name.old_tb_name to new_db_name.new_tb_name
```

### character set

指定编码集

```sql
create database [if not exists] db_name character set gbk
```

### collate

指定校对规则

```sql
create database [if not exists] db_name character set gbk collate utf8_general_ci
```

### unique

唯一约束，可以插入null

### foreign key

```sql
alter table tb小 add constraint tb小_tb大_FK foreign key(col_name小) references tb大(col_name大)
```

> 《阿里巴巴开发手册》中规定：
>
> - 【强制】不得使用外键与级联，一切外键概念必须在应用层解决。
>
>
> **说明**：以学生和成绩的关系为例，学生表中的 student_id 是主键，那么成绩表中的 student_id 则为外键。如果更新学生表中的 student_id，同时触发成绩表中的 student_id 更新，即为级联更新。外键与级联更新适用于单机低并发，不适合分布式、高并发集群;级联更新是强阻塞，存在数据库更新风暴的风 险;外键影响数据库的插入速度。

外键的好处：

- 保证了数据库数据的一致性和完整性；
- 级联操作方便，减轻了程序代码量。
- 如果系统不涉及分不分表，并发量不是很高的情况还是可以考虑使用外键的。

外键的缺点：

- 增加了复杂性，每次做DELETE 或者UPDATE都必须考虑外键约束，会导致开发的时候很痛苦；测试数据极为不方便。
- 外键的主从关系是定的，假如那天需求有变化，数据库中的这个字段根本不需要和其他表有关联的话就会增加很多麻烦。
- 外键还会因为需要请求对其他表内部加锁而容易出现死锁情况。
- 对分库分表不友好，因为分库分表下外键是无法生效的。

### insert

```sql
//插入多行数据
insert into tb_name values(value1,value2,…),(value1,value2,…),…

//向指定列插入数据
insert into tb_name(column1,column2,…) values(value1,value2,…)
```

### select

```sql
//行顺序为书写顺序，序号为select语句执行顺序：
(5) select col_name,…
(1) from tb_name,…
(2) [where…]
(3) [group by…]
(4) [having…]
(6) [order by…]

//MySQL中0表示false，非0表示true。
select * from tb_name where 1

//计算表达式的值
select 3*2
```

### select into

从一个表中选取数据，然后把数据插入另一个表中。常用于创建表的备份复件或者用于对记录进行存档。

```sql
SELECT column_name INTO new_table_name FROM old_tablename
```

### distinct

过滤相同的记录，仅当两行数据一模一样时才会当作重复删除其中一行。

```sql
select distinct clo_name from tb_name
```

### <=>

安全的等于，可用于搜寻值为null的。

### in

​		in()后面的子查询，是返回结果集的，换句话说执行次序和exists()不一样。子查询先产生结果集，然后主查询再去结果集里去找符合要求的字段列表去，符合要求的输出，反之则不输出。相当于多次查询。in 不会使用索引搜索，会全表扫描。

### exists

​		exists()后面的子查询被称做相关子查询 ，不返回列表的值，只返回一个ture或false的结果。先运行主查询一次 ，再去子查询里查询与其对应的结果，如果是ture则输出，反之则不输出。再根据主查询中的每一行去子查询里去查询。相当于查询筛选。

​		如果查询的两个表大小相当，那么用 in 和 exists 差别不大。

​		如果两个表中一个表大，另一个是表小，那么 IN 适合于外表大而子查询表小的情况，EXISTS 适合于外表小而子查询表大的情况。

​		如果两个表中一个表大，另一个是表小，EXISTS 适合于外表小而子查询表大的情况。

### ifnull

ifnull(参数a，参数b)，若参数a不为null，则使用参数a，否则使用参数b。

```sql
select ifnull(
    (select distinct Salary from Employee order by Salary desc limit 1 offset 1),
    null
) as SecondHighestSalary 
```

### case

```sql
//统计胜负
select Date As 比赛日期, SUM(case when Win=’胜’ then 1 else 0 end) 胜, SUM(case when Win=’负’ then 1 else 0 end) 负 from result group by Date
```

### if

if(条件A，参数B，参数C)，若条件A为true，则使用参数B，否则使用参数C。

```sql
//seat座位表，储存学生名字和与他们相对应的座位id，id连续递增，改变相邻俩学生的座位。
//如果学生人数是奇数，则不需要改变最后一个同学的座位。
SELECT CASE 
    WHEN id%2=0 THEN id-1
    ELSE (
        if(id=(select count(id) from seat),id,id+1)
    )
END as id,student FROM seat order by id asc
```

### order

​		对查询结果进行排序。检索出的数据并不是随机显示的。若不排序，数据一般将以它在底层表中出现的顺序显示，这可能是数据最初添加到表中的顺序。若数据随后进行过更新或删除，则该顺序将会受到DBMS重用回收存储空间的方式的影响。因此若不明确控制的话，最终的结果不能（也不应该）依赖该排序顺序。关系数据库设计理论认为，如果不明确规定排列顺序，则不应该假定检索出的数据的顺序有任何意义。     

### limit

```sql
//使用limit关键字实现分页查询
limit (page_num-1)*page_size,page_size
```

### where

​		where 字句在聚合前先筛选记录，也就是说作用在group by 和having前，而 having 子句在聚合后对组记录进行筛选。

### group by

​		根据关键字对数据进行分组查询。group by关键字通常和集合函数一起使用，group by关键字后可接多个列名，从左至右按层次分组。即先按第一个列名分组，然后在第一个列名相同的行中，根据第二个列名进行分组。group by后，返回的组一定要有聚合函数，不然默认返回第一行 。

```sql
//编写一个 SQL 查询，来删除 Person 表中所有重复的电子邮箱，重复的邮箱里只保留 Id 最小 的那个。
delete from Person where id not in(
	//去掉该层子查询会报错，因为修改表的时候不能直接查询被修改的表, 所以需要中间表。
    select single from (
        select min(Id) as single from Person group by Email
    )
    as ids
)
```

### group_concat

```sql
//按照性别进行分组，相同性别的学生合并成一列，列名为people，最终显示两列数据：people和gender。  
select group_concat(name) as people,gender from tb_name group by gender
```

### having

根据判断条件对分组后的结果进行过滤。

### 聚合函数

**count()**：

```sql
//包括了所有的列，相当于行数，不忽略null。
count(*)/count(1)

//计算指定列的总行数，忽略值为null的行
count(col_name)
```

```SQL
//获取 Employee 表中第 n 高的薪水（Salary）。
SELECT DISTINCT e.salary
FROM employee e
WHERE (SELECT count(DISTINCT salary) FROM employee WHERE salary>=e.salary) = N
```

```sql
//部门工资前三高的所有员工
SELECT d.Name AS Department, e.Name AS Employee, e.Salary
FROM Employee e JOIN Department d 
ON e.DepartmentId = d.Id
WHERE(
    SELECT COUNT(DISTINCT Salary)
    FROM Employee
    WHERE (Salary >= e.Salary AND e.DepartmentId = DepartmentId)
) <=3
```

**sum()**：返回指定列的所有值之和，计算时忽略值为null的行。

**avg()**：返回指定列的平均值，计算时忽略值为null的行。

**max()**：返回指定列的最大值，计算时忽略值为null的行。

**min()**：返回指定列的最小值，计算时忽略值为null的行。

**now()**：查看当前时间	

**trim()**：修剪字符串左右两边的空白

**concat()**：拼接字符串	

**DATEDIFF()**：返回两个日期之间的时间，DATEDIFF(datepart,startdate,enddate)，datepart为格式，如dd。

```sql
//给定一个 Weather 表，编写一个 SQL 查询，来查找与之前（昨天的）日期相比温度更高的所有日期的 Id。
select a.Id from Weather a, Weather b where datediff(a.RecordDate,b.RecordDate)=1 and a.Temperature > b.Temperature
```

**charindex()**：通过CHARINDEX如果能够找到对应的字符串，则返回该字符串位置，否则返回0。

```sql
select ename, case charindex(‘A’,ename) when 1 then ’字符A在首位’ when len(ename) then ‘字符A在末位’ when 0 then ’没有字符A’ else ‘字符A在中间’ end 名称类别 from emp;
```

## 查询

### 连接查询

​		内、外连接建立在交叉连接的基础上进行进一步筛选。

#### 交叉连接

​		cross join，返回连接表中所有数据行的笛卡尔积，不带on子句。

```sql
//显式交叉连接
select * from table1 cross join table2
	
//隐式交叉连接
select * from table1,table2
```

#### 内连接

​		inner join，又称：连接/普通连接/自然连接。返回连接表中符合连接条件及查询条件的数据行，从结果表中删除与其他被连接表中没有匹配行的所有行，所以内连接可能会丢失信息。两张表都有的才能显示出来。

```sql
//显式内连接
select * from table1 t1 inner join table2 t2 on t1.field1=t2.field2
select * from table1 as t1 inner join table2 as t2 on t1.field1=t2.field2
//使用inner join关键字，在on子句中设定连接条件。
select table1.*,table2.field as new_name from table1 t1 inner join table2 t2 on t1.field1=t2.field2
	
//隐式内连接
select * from table1 t1,table2 t2 where t1.field1=t2.field2
//不包含inner join关键字和on关键字，在where子句中设定连接条件。
select table1.*,table2.field from table1 t1,table2 t2 where table1.field1=table2.field2
```

```sql
//Employee表包含所有员工，他们的经理也属于员工。每个员工都有一个 Id，此外还有一列对应员工的经理的 Id。查询可以获取收入超过他们经理的员工的姓名。
select a.Name as Employee from Employee a,Employee b where a.Salary>b.Salary and a.ManagerId=b.Id
```

```sql
//编写一个 SQL 查询，查找所有至少连续出现三次的数字。
SELECT DISTINCT a.Num AS ConsecutiveNums FROM Logs a,Logs b,Logs c WHERE
a.Num=b.Num AND
a.Num=c.Num AND
b.Num=c.Num AND
a.Id+1=b.id AND
b.Id+1=c.id
```

#### 外连接

##### 全连接

​		左表和右表都不做限制，所有的记录都显示，去除两表的重复数据，两表不足的地方用null填充。

##### 左外连接查询

​		left join。返回连接表中符合连接条件及查询条件的数据行及写在left outer join左边的表符合查询条件但不符合连接条件的数据行。左边表的所有数据都有显示出来，右边的表数据只显示共同有的那部分，没有对应的部分只能补空显示。

```sql
select * from table1 t1 left outer join table2 t2 on t1.field1=t2.field2 where 查询条件
```

> 例如：查询学生的消费信息，学生的学号等于消费记录的学号是连接条件，没有消费的学生在消费记录中没有学号记录，所以不满足连接条件。但左外查询也会显示没有消费的学生，即所有学生的信息都显示，有消费记录的就显示消费记录。

##### 右外连接查询

​		right join。返回连接表中符合连接条件及查询条件的数据行及写在right outer join右边的表符合查询条件但不符合连接条件的数据行。右边表的所有数据都会显示出来，左边的只会出现共同的那部分，其他的空。

```sql
select * from table1 t1 right outer join table2 t2 on t1.field1=t2.field2 where 查询条件
```

### 联合查询

​		union，合并两条查询语句的查询结果，去掉其中的重复数据行，返回没有重复数据行的查询结果。联合查询的各子查询使用的表结构应该相同，同时两个子查询返回的列也应相同。

```sql
select * from table1 where field1>100 union select * from table2 where field2=100
```

### 嵌套查询

​		在where子句或from子句中又嵌入select查询语句（一般写在where字句）

```sql
//找出选择哪个研究方向的导师最多
select field1 from tb_name where field2=(select max(field2) from tb_name)
select direction,count from (select direction,count(direction) as count from teacher group by direction) as caldirection where count = (select max(count) from (select count(*) as count from teacher group by direction) as a);
```

### 报表查询

​		对数据行进行分组统计，group by子句指定按照哪些字段分组，having子句设定分组查询条件，在报表查询中可以使用SQL函数。

```sql
select …  from … [where…] [ group by … [having… ]] [ order by … ]
```

### 模糊查询

> 《阿里巴巴开发手册》规定：
>
> - 【强制】页面搜索严禁左模糊或者全模糊，如果需要请走搜索引擎来解决。
>
>
> **说明**：索引文件具有 B-Tree 的最左前缀匹配特性，如果左边的值未确定，那么无法使用此索引。

## 优化

- 尽量多使用COMMIT：在程序中尽量多使用COMMIT，这样程序的性能得到提高，需求也会因为COMMIT所释放的资源而减少。

- SQL语句用大写的。因为Orale总是先解析SQL语句，把小写的字母转换成大写的再执行。

- 使用表的别名(Alias)。当在SQL语句中连接多个表时，使用表的别名并把别名前缀于每个Column上。这样一来，就可以减少解析的时间并减少那些由Column歧义引起的语法错误。

- 不要写一些没有意义的查询，如需要生成一个空表结构，请使用create。

    ```sql
    select col1,col2 into #t from t where 1=0 
    //这类代码不会返回任何结果集，但是会消耗系统资源，应改成：    
    create table #t(...)
    ```

- 在新建临时表时，如果一次性插入数据量很大，那么可以使用 select into 代替 create table，避免造成大量log，以提高速度。如果数据量不大，为了缓和系统表的资源，应先create table，然后insert。

- 如果使用到了临时表，在存储过程的最后务必将所有的临时表显式删除，先 truncate table ，然后 drop table ，这样可以避免系统表的较长时间锁定。   

- 避免频繁创建和删除临时表，以减少系统表资源的消耗。临时表并不是不可使用，适当地使用它们可以使某些例程更有效，例如，当需要重复引用大型表或常用表中的某个数据集时。对于一次性事件，最好使用导出表。 

- 与临时表一样，游标并不是不可使用。对小型数据集使用 FAST_FORWARD 游标通常要优于其他逐行处理方法，尤其是在必须引用几个表才能获得所需的数据时。在结果集中包括“合计”的例程通常要比使用游标执行的速度快。如果开发时间允许，基于游标的方法和基于集的方法都可以尝试一下，看哪一种方法的效果更好。

- 使用基于游标的方法或临时表方法之前，应先寻找基于集的解决方案来解决问题，基于集的方法通常更有效。 

- 尽量避免使用游标，因为游标的效率较差，如果游标操作的数据超过1万行，那么就应该考虑改写。    

- 尽量避免大事务操作，提高系统并发能力。

- 尽量避免向客户端返回大数据量，若数据量过大，应该考虑相应需求是否合理。

- 用TRUNCATE替代DELETE：当删除表中的记录时,在通常情况下, 回滚段(ROLLBACK SEGMENTS ) 用来存放可以被恢复的信息. 假如你没有COMMIT事务,ORACLE会将数据恢复到删除之前的状态(准确地说是恢复到执行删除命令之前的状况) 而当运用TRUNCATE时, 回滚段不再存放任何可被恢复的信息.当命令运行后,数据不能被恢复.因此很少的资源被调用,执行时间也会很短. (注意: TRUNCATE只在删除全表适用,TRUNCATE是DDL不是DML) 。

- 不要使用 select * from t ，用具体的字段列表代替*，不要返回用不到的任何字段。    

- 当只要一行数据时使用limit 1，MySQL数据库引擎会在找到一条数据后停止搜索，而不是继续往后查少下一条符合记录的数据。

- 尽量使用数字型字段，若只含数值信息的字段尽量不要设计为字符型，这会降低查询和连接的性能，并会增加存储开销。这是因为引擎在处理查询和连接时会逐个比较字符串中每一个字符，而对于数字型而言只需要比较一次就够了。

- 尽可能的使用varchar代替char，因为首先变长字段存储空间小，可以节省存储空间，    其次对于查询来说，在一个相对较小的字段内搜索效率显然要高些。    

- 使用 ENUM 而不是 VARCHAR。如果你有一个字段，你知道这些字段的取值是有限而且固定的，那么应该使用 ENUM 而不是VARCHAR。   

- 用IN来替换OR 

- 用EXISTS替代IN、用NOT EXISTS替代NOT IN： 在许多基于基础表的查询中,为了满足一个条件,往往需要对另一个表进行联接.在这种情况下, 使用EXISTS(或NOT EXISTS)通常将提高查询的效率. 在子查询中,NOT IN子句将执行一个内部的排序和合并. 无论在哪种情况下,NOT IN都是最低效的 。 

    ```sql
    select num from a where num in(select num from b)    
    //改成：    
    select num from a where exists(select 1 from b where num=a.num) 
    ```

- 用EXISTS替换DISTINCT

- in 和 not in 也要慎用，否则会导致全表扫描。对于连续的数值，能用between或and就不要用 in。

    ```sql
    select id from t where num in(1,2,3)    
    //改成：
    select id from t where num between 1 and 3   
    ```

- 用>=替代> 

- 用Where子句替换HAVING子句

- 对查询进行优化，应尽量避免全表扫描，首先应考虑在 where 及 order by 涉及的列上建立索引。    

- 尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描。可以将值设置默认值0，确保表中num列没有null值。

- 尽量避免在 where 子句中使用 or 来连接条件，否则将导致引擎放弃使用索引而进行全表扫描，可以用union all。

    ```sql
    select id from t where num=10 or num=20    
    //可以改成：    
    select id from t where num=10 
    union all 
    select id from t where num=20    
    ```

- 尽量避免在 where 子句中=号左边对字段进行表达式操作、函数运算、算数运算，这将导致引擎放弃使用索引而进行全表扫描。

    ```sql
    select id from t where num/2=100    
    //改成：  
    select id from t where num=100*2 
    ```

    ```sql
    select id from t where substring(name,1,3)='abc'
    //改成： 
    select id from t where name like 'abc%'    
    ```

- like模糊查询别用通配符开头，会导致全表扫描。

- 在使用索引字段作为条件时，如果该索引是复合索引，应遵循最左前缀原则，必须使用到该索引中的第一个字段作为条件时才能保证系统使用该索引，否则该索引将不会被使用，并且应尽可能的让字段顺序与索引顺序相一致。   

- 并不是所有索引对查询都有效，SQL是根据表中数据来进行查询优化的，当索引列有大量数据重复时，SQL查询可能不会去利用索引。如一表中有字段sex，male、female几乎各一半，那么即使在sex上建了索引也对查询效率起不了作用。    

- 索引并不是越多越好，索引固然可以提高相应的 select 的效率，但同时也降低了 insert 及 update 的效率，因为 insert 或 update 时有可能会重建索引，所以怎样建索引需要慎重考虑，视具体情况而定。一个表的索引数最好不要超过6个，若太多则应考虑一些不常使用到的列上建的索引是否有必要。    

## 事务

​		事务是逻辑上的一组操作，组成这组操作的各单元，需全部成功或全部不成功。数据库中的数据是共享资源，当一个数据库允许多个用户同时访问的时候，数据库系统就必须支持并发控制。在数据库中，事务是并发控制的基本单位，而对数据库数据并发访问时，事务的隔离性，保证了并发访问数据库时，数据的正确性和一致性。若不考虑事务的隔离性，就会发生数据不一致的情况。避免由于隔离性不好产生问题，SQL标准委员会对数据库共定义了四种隔离级别。

### 特性

**原子性 Atomicity**：事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。 

**一致性 Consistency**：事务必须使数据库从一个一致性状态变换到另外一个一致性状态

**隔离性 Isolation**：多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰，多个并发事务之间要相互隔离。

**持久性 Durability**：一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来即使数据库发生故障（软件故障）也不应该对其有任何影响。

### 隔离级别

​		设置隔离级别之后，并不是不能并发，而是设置并发的时候，一个事务的修改数据什么时候才能被另一个事务读到，但彼此的逻辑操作没有影响。如：绝对读到、提交的才能读到、提交不提交更新的数据都读不到、提交不提交增删的数据都读不到。

**serializable**：可避免脏读、不可重复读、虚读情况的发生。串行化，牺牲了并发，换取绝对的隔离。不能并发执行两个事务，该串行化针对行锁，不同的事务可以并发执行。

**repeatable read**：可重复读。默认隔离级别。可避免脏读、不可重复读情况的发生，无法避免虚读。MySQL中的repeatable read在此基础上不允许虚读。例如要取出当前账单+上月余额+当前余额核账，一个事务中取出了当前账单+上月余额，此时若未设隔离级别，又有账单扣款了，会导致当前余额读到的不一致。

**read committed**：读已提交。可避免脏读情况发生，无法避免不可重复读、虚读。

**read uncommitted**：读未提交。最低级别。脏读、不可重复读、虚读均无法保证。

### 隔离性差产生的问题

**脏读 dirty reads**：一个事务读取了另一个事务修改了但未提交的数据

**不可重复读 non-repeatable reads**：一个事务针对同一条记录，多次读取的结果不一样。重新读取前面读取过的数据，发现该数据已经被另一个已提交的事务修改过。

**虚读/幻读 phantom read**：一个事务前后读取的结果的数据量不一致，两次读之间数据被其他的事务删除或插入。

## 范式

**第一范式**：每列保证原子性，不可再分割。

**第二范式**：在1NF的基础上，要求每个非主属性完全依赖于候选键，不存在部分依赖关系。

**第三范式**：在2NF的基础上，要求每个非主属性对任何候选关键字都不存在依赖传递。

**BC范式**：在3NF的基础上，主属性不依赖于主属性。

## Question

>*Q. 一个表3个字段，有很多数据是3个字段都重复的，全部删除保留一条，怎么写。*
>
>```sql
>delete from 表名 where (列1, 列2, 列3) in 
>(select 列1, 列2, 列3 from 表名 group by 列1, 列2, 列3 having count(*)>1)
>and id not in (select min(id) from 表名 group by 列1, 列2, 列3 having count(*)>1)
>```
