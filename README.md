# MySQL

docsify serve mysql

MySQL密码：alice1234



# 一  概念

> **SQL语句是通用的**
>
> **所有SQL语句以分号；结尾**
>
> **SQL语句不区分大小写**
>
> **数据库中的有一条命名规范：所有的标识符都是全部小写，单词和单词之间使用下划线进行衔接。**
>
> xxxx.sql这种文件被称为sql脚本文件。
> sql脚本文件中编写了大量的sql语句。
> 执行sql脚本文件的时候，该文件中所有的sql语句会全部执行！
> 批量的执行SQL语句，可以使用sql脚本文件。
> 在mysql当中执行sql脚本：mysql> source D:\course\03-MySQL\document\vip.sql



> 数据库（DataBase，DB）：按照一定格式存储数据的一些文件的组合
>
> 数据库管理系统（DataBaseManagement，DBMS）
>
> SQL：结构化查询语言
>
> DBMS-- 执行-- >SQL-- 操作-- >DB
>
> mysql数据库启动的时候，默认端口号3306
>
> MySQL数据库的字符编码方式为utf8



> win系统启动和关闭MySQL服务
>
> net stop 服务名称；（MySQL）
>
> net start 服务名称；



> 登录mysql
>
> * 打开终端 mysql -u root -p
> * 输入密码 alice1234



> mysql的常用命令(不见分号不执行；或者 \c 终止一条命令)
>
> * 退出：exit
> * 查看MySQL中数据库：show databases;（必须加英文分号）
> * 使用数据库：use 数据库名称;
> * 创建数据库：create database 数据库名称;
> * 使用数据库：use 数据库名;
> * 删除数据库：drop database if exists 数据库名; 
> * 查看数据库中的表：show tables; 
> * 导入sql数据库：source 文件路径（路径中不能有中文,;用/）
> * 查看表中的数据：select * from 表名;
>
> * 不看表中数据，只看表的结构：desc 表名; (describe 表名)
> * 查看MySQL数据库版本号：select version();
> * 查看当前使用的数据库：select database(); 



> MySQL默认数据库四个
>
> | information_schema   |
> | mysql            			      |
> | performance_schema |
> | sys             				 	 |



> 数据库最基本的单元是表table，因为表比较直观
>
> 任何一张表有行和列
>
> * 行（row）称为数据/记录
> * 列（column）称为字段
>
> 每一个字段都有字段名、数据类型、约束等属性（唯一性约束：该字段不能重复）



> **SQL 语句分类（背）**
>
> * DQL：数据查询语言（select ...）
> * DML：数据操作语言（表的数据  insert  delete  update）
> * DDL：数据定义语言（DDL主要操作是表的结构，不是表的数据  create增  drop删除  alter修改）
> * TCL：事务控制语言（包括事务提交commit  事务回滚 rollback）
> * DCL：数据控制语言（授权grant 撤销权限 revoke）



# 二 DQL 

**select 语句不会修改，只查询**

**在所有的数据库当中，字符串统一使用单引号括起来，单引号是标准，双引号在oracle数据库中用不了。但是在mysql中可以使用。**

## 2.1 简单查询

> 查询一个字段：select 字段名 from 表名; 
>
> 查询两个，多个字段：使用逗号隔开 
>
> 查询所有：每个字段都写 或者 使用“ * ”（效率低，可读性差）
>
> 给查询的列起别名：select 字段原名 as 字段别名 from  表名;  (只是将查询解挂列名显示为别名，原表列名不变) 或者  select 字段原名  字段别名 from  表名; （**别名有空格，必须加单引号或双引号**）
>
> 列参与数学运算： select 字段名*12 as 别名 from 表名; (字段可以使用数学表达式，**中文需要单引号**)



## 2.2 条件查询

语法格式：select 字段1,字段2 from 表名 where 条件; 

在数据库中null不能使用等号，因为数据库中的null代表什么也没有，它不是一个值

and 和or 同时出现，and优先级较高

in 不是一个区间，后面是具体的值

![条件查询](图表\条件查询.png)



## 2.3 排序数据

> 单一字段排序：select * from 表名 order by 字段名1,字段名2; 
>
> * 默认升序
> * order by 必须在where语句后面
> * 按照多字段排序，先字段名1再字段名2



> 手动指定排序
>
> * select * from 表名 order by 字段名1 **asc**; 由小到大
> * select * from 表名 order by 字段名1 **desc**; 由大到小



> 多个字段排序：select * from 表名 order by 字段名1 desc,字段名2 desc; 
>
> 使用字段的位置来排序: elect * from 表名 order by 6;（6为字段位置，不建议，数字含义不明确）



## 2.4 数据处理函数/单行处理函数

> select 函数(字段名) from 表名 
>
> select * from 表名 where 条件函数=函数()
>
> eg: select * from emp where job=upper('manager')
>
> 数据处理函数又被称为单行处理函数，特点：输入一行输出一行



![](图表\数据处理函数1.png)

![](图表\数据处理函数2.png)



> trim 会去首尾空格，不会去除中间的空格
>
> str_to_date (必须严格按照标准输出)
>
> * 一 与数据库的格式匹配上   select * from emp where HIREDATE='1981-02-20';
> * 二 将字符串转换成 date 类型  select * from emp where HIREDATE=str_to_date('1981-02-20','%Y-%m-%d');
>
> now() 获得当前时间
>
> 查询员工薪水加入千分位和保留两位小数 select empno, ename, Format(sal, 2) from emp;
>
> 四舍五入   select round(123.56);
>
> 生成随机数  select rand();
>
> 随机抽取记录数 select * from emp order by rand() limit 2; order by 必须写上



> case … when … then …..else …end
>
> * select empno, ename, job, sal, case job when 'MANAGER' then sal*1.1 when 'SALESMAN' then sal*1.5 end as newsal from emp;   如果 job 为 MANAGERG 薪水上涨 10%,如果 job 为 SALESMAN 工资上涨 50%
> * select sal ,case job when 'salesman' then sal*1.1 when 'clerk' then sal*1.2 else sal end as  new_sal from emp;  其他的工资不动，需要添加 else 



> ifnull
>
> select ifnull(comm,0) from emp; 如果 comm 为 null 就替换为 0 
>
> 在 SQL 语句当中若有 NULL 值参与数学运算，计算结果一定是 NULL 为了防止计算结果出现 NULL，建议先使用 ifnull 空值处理函数预先处理。



## 2.5 分组函数/聚合函数/多行处理函数

> 分组函数自动忽略空值，不需要手动的加 where 条件排除空值。 
>
> select count(*) from emp where xxx;   符合条件的所有记录总数。 
>
> select count(comm) from emp;    comm 这个字段中不为空的元素总数。



| 函数  | 解释       | 注意                                                         |
| ----- | ---------- | ------------------------------------------------------------ |
| count | 取得记录数 | Count(*)表示取得所有记录，忽略 null，为 null 的值也会取得；采用 count(字段名称)，不会取得为 null 的记录 |
| sum   | 求和       | Sum 可以取得某一个列的和，null 会被忽略；两列求和，需要将 null 值转换成 0，无法计算 |
| avg   | 取平均     |                                                              |
| max   | 取最大的数 |                                                              |
| min   | 取最小的数 |                                                              |



> 分组查询
>
> group by
>
> * select job, sum(sal) from emp **group by** job
>
> * 如果使用了 order by，order by 必须放到 group by 后面
>
> * 在 SQL 语句中若有 group by 语句，那么在 select 语句后面只能跟分组函数+参与分组的字段。
>
> having
>
> * 如果想对分组数据再进行过滤需要使用 having 子句

## 2.6 查询结果去重

原表数据不会被修改，只是查询结果去重。
去重需要使用一个关键字：distinct

distinct只能出现在所有字段的最前方。

distinct出现在job,deptno两个字段之前，表示两个字段联合起来去重。

	mysql> select distinct job from emp;
		
	mysql> select distinct job,deptno from emp;// distinct出现在job,deptno两个字段之前，表示两个字段联合起来去重。



## 2.7 select 语句总结

![](C:\ubuntu\net_file\mysql\图表\DQL.png)

原则：能在 where 中过滤的数据，尽量在 where 中过滤，效率较高。having 的过滤是专门对分组之后的数据进行过滤的。

# 三 DQL

## 3.1 连接查询（重要）

> 从一张表中单独查询，称为单表查询
>
> 多张表联合查询数据，称为连接查询
>
> 分类：
>
> * SQL92 1992年出现的语法
> * SQL99 1999年出现的语法
>
> 表连接的方式分类：
>
> * 内连接：等值连接；非等值连接；自连接
> * 外连接：左外连接（左连接）；右外连接（右连接）
> * 全连接（很少使用）



### 3.1.1 笛卡尔积现象

> 当两张表进行连接查询，没有任何条件限制，最终查询结构条数是两张表条数的乘积，这种现象被称为：笛卡尔积现象。（这是一个数学现象。）
>
> 如何避免笛卡尔积现象：
>
> * 连接时加条件，满足这个条件的记录被筛选出来
> * 但是匹配的过程中，匹配的次数没有减少。
> * 通过笛卡尔积现象得出，表的连接次数越多效率越低，尽量避免表的连接次数。
> * **表起别名**。很重要。效率问题。



### 3.1.2 内连接

> 内连接的等值连接
>
> 案例：查询每个员工所在部门名称，显示员工名和部门名？
>
> * SQL92语法(缺点：结构不清晰，表的连接条件，和后期进一步筛选的条件，都放到了where后面)
>   		select 
>     			e.ename,d.dname
>     		from
>     			emp e, dept d
>     		where
>     			e.deptno = d.deptno;
>
> * SQL99语法：join 后是表  on后加条件 
>
>   ​	select 
>   ​	     e.ename,d.dname
>   ​	from
>   ​		emp e
>   ​	inner join
>   ​		dept d
>   ​	on 
>   ​		e.deptno = d.deptno;
>
> SQL99优点：表连接的条件是独立的，连接之后，如果还需要进一步筛选，再往后继续添加where
>
> //inner可以省略（带着inner可读性更好！一眼就能看出来是内连接）
>
> SQL99语法：
> 			select  ...
> 			from
> 				a
> 			join
> 				b
> 			on
> 				a和b的连接条件
> 			where
> 				筛选条件



> 内连接的非等值连接
>
> 	select 
> 	 	e.ename, e.sal, s.grade
> 	from
> 		emp e
> 	join
> 		salgrade s
> 	on
> 		e.sal between s.losal and s.hisal; // 条件不是一个等量关系，称为非等值连接。



> 内连接之自连接
>
> 技巧：一张表看做两张表。
>
> 内连接的特点：完成能够匹配上这个条件的数据查询出来。
>
> 内连接：（A和B连接，AB两张表没有主次关系。平等的。）



### 3.1.3 外连接

> 在外连接当中，两张表连接，产生了主次关系。



> 右连接  
>
> right: 表示将join关键字右边的这张表看成主表，主要是为了将这张表的数据全部查询出来，捎带着关联查询左边的表。
>
> // outer是可以省略的，带着可读性强。
> ``select` 
> 	`e.ename,d.dname`
> `from`
> 	`emp e` 
> `right outer join` 
> 	`dept d`
> `on`
> 	`e.deptno = d.deptno;``
>
> 左连接
> select` 
> 	`e.ename,d.dname`
> `from`
> 	`dept d` 
> `left outer join` 
> 	`emp e`
> `on`
> 	`e.deptno = d.deptno;



> 带有right的是右外连接，又叫做右连接。
> 带有left的是左外连接，又叫做左连接。
> 任何一个右连接都有左连接的写法。
> 任何一个左连接都有右连接的写法。
>
> 外连接的查询结果条数一定是 >= 内连接的查询结果条数



> 三张表，四张表怎么连接？
> 	语法：

		select 
			...
		from
			a
		join
			b
		on
			a和b的连接条件
		join
			c
		on
			a和c的连接条件
		right join
			d
		on
			a和d的连接条件



>
> 一条SQL中内连接和外连接可以混合。都可以出现



## 3.2 子查询

> select语句中嵌套select语句，被嵌套的select语句称为子查询
>
> **where子句中不能直接使用分组函数。**
>
> 子查询都可以出现在哪里呢？



	select
		..(select).
	from
		..(select).
	where
		..(select).



> where子句中的子查询
>
> * select ename,sal from emp where sal > (select min(sal) from emp);
>
> from 子句中的子查询
>
> * from后面的子查询，可以将子查询的查询结果当做一张临时表。（技巧）
> * select 
>   			t.*, s.grade
>     		from
>     			(select job,avg(sal) as avgsal from emp group by job) t
>     		join
>     			salgrade s
>     		on
>     			t.avgsal between s.losal and s.hisal;
>   		
>
> select后面出现的子查询（了解）
>
> * 对于select后面的子查询来说，这个子查询只能一次返回1条结果(一列)，多于1条，就报错了。
> * select 
>   		e.ename,e.deptno,(select d.dname from dept d where e.deptno = d.deptno) as dname 
>     	from 
>     		emp e;



> union合并查询结果集
>
> * union在进行结果集合并的时候，要求两个结果集的列数相同。
> * union把乘法变成了加法运算
> * 列和列的数据类型不一致时，union合并，MYSQL可以，oracle语法严格 ，不可以，要求：结果集合并时列和列的数据类型也要一致。



	select ename,job from emp where job = 'MANAGER'
	union
	select ename,job from emp where job = 'SALESMAN';

union的**效率要高**一些。对于表连接来说，每连接一次新表，则匹配的次数满足笛卡尔积，成倍的翻。。。但是**union可以减少匹配的次数**。在减少匹配次数的情况下，还可以完成两个结果集的拼接



## 3.3 limit 的使用（重要）

> limit作用：将查询结果集的一部分取出来。通常使用在分页查询当中。
>
> 分页的作用是为了**提高用户的体验**，因为一次全部都查出来，用户体验差。
>
> **完整用法**：limit startIndex, length
> 		startIndex是起始下标，length是长度。起始下标从0开始。
>
> **缺省用法**：limit 5; 这是取前5.
>
> **mysql当中limit在order by之后执行**



> 分页
>
> * 每页显示pageSize条记录：第pageNo页：limit (pageNo - 1) * pageSize  , pageSize



## 3.4 关于DQL语句的大总结：

	select 
		...
	from
		...
	where
		...
	group by
		...
	having
		...
	order by
		...
	limit
		...
	
	执行顺序：
		1.from
		2.where
		3.group by
		4.having
		5.select
		6.order by
		7.limit..



# 四 表

## 4.1 表的创建，删除(DDL)

> 建表的语法格式：(建表属于DDL语句，DDL包括：create drop alter)
>
> 表名：建议以t_ 或者 tbl_开始，可读性强。见名知意。
> 字段名：见名知意。
> 表名和字段名都属于标识符。
>
> 	create table 表名(字段名1 数据类型, 字段名2 数据类型, 字段名3 数据类型);
> 			
> 	create table 表名(
> 		字段名1 数据类型, 
> 		字段名2 数据类型, 
> 		字段名3 数据类型
> 	);



> 删除表：
>
> * drop table 表名; // 当这张表不存在的时候会报错
>
> * drop table if exists t_student; // 如果这张表存在的话，删除



## 4.2 mysql中的数据类型

| 数据类型         | 解释                                                         | 优点                         | 缺点                           |
| ---------------- | ------------------------------------------------------------ | ---------------------------- | ------------------------------ |
| varchar(最长255) | 可变长度的字符串<br/>比较智能，节省空间。<br/>会根据实际的数据长度动态分配空间。 | 节省空间                     | 需要动态分配空间，速度慢。     |
| char(最长255)    | 定长字符串<br/>不管实际的数据长度是多少。<br/>分配固定长度的空间去存储数据。<br/>使用不恰当的时候，可能会导致空间的浪费。 | 不需要动态分配空间，速度快。 | 使用不当可能会导致空间的浪费。 |
| int(最长11)      | 数字中的整数型。等同于java的int。                            |                              |                                |
| bigint           | 数字中的整数型。等同于java的long。                           |                              |                                |
| float            | 单精度浮点型数据                                             |                              |                                |
| double           | 双精度浮点型数据                                             |                              |                                |
| date             | 短日期类型，只包括年月日信息。                               | 默认格式：%Y-%m-%d           |                                |
| datetime         | 长日期类型，包括年月日时分秒信息。                           | 默认格式：%Y-%m-%d %h:%i:%s  |                                |
| clob             | 字符大对象<br/>最多可以存储4G的字符串。<br/>超过255个字符的都要采用CLOB字符大对象来存储。 |                              |                                |
| blob             | 二进制大对象<br/>专门用来存储图片、声音、视频等流媒体数据。<br/>往BLOB类型的字段上插入数据的时候，需要使用IO流才行。 |                              |                                |



> varchar和char我们应该怎么选择？
>
> * 性别字段你选什么？因为性别是固定长度的字符串，所以选择char。
> * 姓名字段你选什么？每一个人的名字长度不同，所以选择varchar。



## 4.3 insert 语句

插入数据insert （DML）

* 语法格式：
  		insert into 表名(字段名1,字段名2,字段名3...) values(值1,值2,值3);

* 字段名和值要一一对应。数量要对应。数据类型要对应。

* insert语句但凡是执行成功了，那么必然会多一条记录。没有给其它字段指定值的话，默认值是NULL。
* 可使用default指定默认值
* insert语句中的“字段名”可以省略；前面的字段名省略的话，等于都写上了！所以值也要都写上！

	drop table if exists t_student;
	create table t_student(
		no int,
		name varchar(32),
		sex char(1) default 'm',//指定默认值
		age int(3),
		email varchar(255)
	);



### 4.3.1 insert插入日期

> (不常用)数字格式化：format
>
> (不常用)格式化数字：format(数字, '格式')  :select ename,format(sal, '$999,999') as sal from emp;



> str_to_date：将字符串varchar类型转换成date类型
>
> * 语法格式：str_to_date('字符串日期', '日期格式')
>
> * mysql的日期格式：%Y	年；%m 月；%d 日；%h	时；%i	分；%s	秒
> * str_to_date函数可以把字符串varchar转换成日期date类型数据，通常使用在插入insert方面，因为插入的时候需要一个日期类型的数据，需要通过该函数将字符串转换成date。
> * 如果你提供的日期字符串是%Y-%m-%d格式，str_to_date函数就不需要了，可自动转换
>
> 	insert into t_user(id,name,birth) values(1, 'zhangsan', str_to_date('01-10-1990','%d-%m-%Y'));



> date_format：将date类型转换成具有一定格式的varchar字符串类型。
>
> * date_format(日期类型数据, '日期格式')
>
> * 通常使用在查询日期方面。设置展示的日期格式。
>
> * 		mysql> select id,name,birth from t_user;
>         		+------+----------+------------+
>         		| id   | name     | birth      |
>         		+------+----------+------------+
>         		|    1 | zhangsan | 1990-10-01 |
>         		+------+----------+------------+
>         		以上的SQL语句实际上是进行了默认的日期格式化，自动将数据库中的date类型转换成varchar类型。并且采用的格式是mysql默认的日期格式：'%Y-%m-%d'
>
> 	select id,name,date_format(birth,'%Y/%m/%d') as birth from t_user;



> 在mysql当中获取系统当前时间:
>
> * now() 函数，并且获取的时间带有：时分秒信息，是datetime类型的。



### 4.3.2 insert 插入多条数据

一次可以插入多条记录

语法：insert into t_user(字段名1,字段名2) values(),(),(),();

	insert into t_user(id,name,birth,create_time) values
		(1,'zs','1980-10-11',now()), 
		(2,'lisi','1981-10-11',now()),
		(3,'wangwu','1982-10-11',now());



> 将查询结果插入到一张表当中（了解）
>
> * insert into dept_bak select * from dept; //很少用



## 4.4 修改update 删除数据 delete（DML）

> 修改update
>
> * 语法格式：
>   	update 表名 set 字段名1=值1,字段名2=值2,字段名3=值3... where 条件;
> * 没有条件限制会导致所有数据全部更新。



> 删除数据 delete
>
> * 语法格式:
>   		delete from 表名 where 条件;
> * 没有条件，整张表的数据会全部删除



## 4.5 快速创建表(了解)

	create table emp2 as select * from emp;

原理：
	将一个查询结果当做一张表新建！
	这个可以完成表的快速复制！
	表创建出来，同时表中的数据也存在了！



## 4.6 快速删除表中的数据(truncate必须掌握)

	delete from dept_bak; //删除dept_bak表中的数据，比较慢，可以再恢复数据
	truncate table dept_bak;//不支持回滚；快速
	drop table 表名; // 这不是删除表中的数据，这是把表删除。

| 语句         | 原理                                                         | 优点             | 缺点                           | 属于    |
| ------------ | ------------------------------------------------------------ | ---------------- | ------------------------------ | ------- |
| delete语句   | 表中的数据被删除了，但是这个数据在硬盘上的真实存储空间不会被释放 | 删除效率比较低。 | 支持回滚，后悔了可以再恢复数据 | DML语句 |
| truncate语句 | 效率比较高，表被一次截断，物理删除。                         | 不支持回滚。     | 快速。                         | DDL操作 |

## 4.7 对表结构的增删改

> 对表结构的修改:添加一个字段，删除一个字段，修改一个字段
>
> 对表结构的修改需要使用：alter，属于DDL语句
>
> DDL包括：create drop alter



> 对表结构的增删改不重要：
>
> * 第一：在实际的开发中，需求一旦确定之后，表一旦设计好之后，很少的进行表结构的修改。因为开发进行中的时候，修改表结构，成本比较高。修改表的结构，对应的java代码就需要进行大量的修改。成本是比较高的。这个责任应该由设计人员来承担！
>
> * 第二：由于修改表结构的操作很少，所以我们不需要掌握，如果有一天真的要修改表结构，你可以使用工具！！！！
>   修改表结构的操作是不需要写到java程序中的。实际上也不是java程序员的范畴。



# 五 约束（非常重要）


定义（constraint）：在创建表的时候，给表中的字段加上一些约束，来保证这个表中数据的完整性、有效性

作用：保证：表中的数据有效

包括

* 非空约束：not null
* 唯一性约束: unique
* 主键约束: primary key （简称PK）
* 外键约束：foreign key（简称FK）
* 检查约束：check（mysql不支持，oracle支持）



## 5.1 非空约束：not null

非空约束not null约束的字段不能为NULL，not null只有列级约束，没有表级约束

	create table t_vip(
		id int,
		name varchar(255) not null  // not null只有列级约束，没有表级约束！
	);



## 5.2 唯一性约束: unique

唯一性约束unique约束的字段不能重复，但是可以为NULL。可以列级约束，表级约束。

使用表级约束：需要给多个字段联合起来添加某一个约束的时候，需要使用表级约束。

在mysql当中，如果一个字段同时被not null和unique约束的话，该字段自动变成主键字段。（注意：oracle中不一样）

	create table t_vip(
		id int,
		name varchar(255) unique,
		email varchar(255)
	);

表级约束，两个字段（多个）联合起来具有唯一性

			drop table if exists t_vip;
			create table t_vip(
				id int,
				name varchar(255),
				email varchar(255),
				unique(name,email) // 约束没有添加在列的后面，这种约束被称为表级约束。
			);



## 5.3 主键约束: primary key （PK）（掌握）

主键约束：就是一种约束。
主键字段：该字段上添加了主键约束，这样的字段叫做：主键字段
主键值：主键字段中的每一个值都叫做：主键值。

**主键值是每一行记录的唯一标识，是每一行记录的身份证号**

主键的特征：not null + unique（主键值不能是NULL，同时也不能重复）

**任何一张表都应该有主键，没有主键，表无效**

		drop table if exists t_vip;
		// 1个字段做主键，叫做：单一主键
		create table t_vip(
			id int primary key,  //列级约束
			name varchar(255)
		);

		drop table if exists t_vip;
		create table t_vip(
			id int,
			name varchar(255),
			primary key(id)  // 表级约束
		);

		drop table if exists t_vip;
		// id和name联合起来做主键：复合主键！！！！
		create table t_vip(
			id int,
			name varchar(255),
			email varchar(255),
			primary key(id,name)
		);



在实际开发中不建议使用：复合主键（比较复杂）。建议使用单一主键！
因为主键值存在的意义就是这行记录的身份证号，只要意义达到即可，单一主键可以做到。
**一张表，主键约束只能添加1个**。（主键只能有1个。）

主键值建议使用：int，bigint，char等类型。

不建议使用：varchar来做主键。主键值一般都是数字，一般都是定长的



主键除了：单一主键和复合主键之外，其他分类：

* 自然主键：主键值是一个自然数，和业务没关系。（自然主键使用比较多）
* 业务主键：主键值和业务紧密关联，例如拿银行卡账号做主键值。（当业务发生变动的时候，可能会影响到主键值）



在mysql当中，有一种机制，可以自动维护一个主键值

		drop table if exists t_vip;
		create table t_vip(
			id int primary key auto_increment, //auto_increment表示自增，从1开始，以1递增！
			name varchar(255)
		);



## 5.4 外键约束（foreign key，简称FK）（非常重要）

外键约束：一种约束（foreign key）
外键字段：该字段上添加了外键约束
外键值：外键字段当中的每一个值。

子表：引用的表；父表：被引用的表；

子表中的外键引用的父表中的某个字段，被引用的这个字段**不一定是主键，但至少具有unique约束**。

外键值可以为NULL

	create table class(
		classno int prinmary key,
		classname varchar(255)
	);
	create table t_student(
		no int primary key auto_increment,
		name varchar(255),
		cno int,
		foreign key(cno) references t_class(classno)//外键约束
	);


# 六 存储引擎（了解）

存储引擎是MySQL中特有的一个术语，其它数据库中没有。（Oracle中有，但是不叫这个名字）
实际上存储引擎是一个表存储/组织数据的方式。不同的存储引擎，表存储数据的方式不同。

给表添加/指定“存储引擎”：

	show create table t_student;
	
	可以在建表的时候给表指定存储引擎。
	CREATE TABLE `t_student` (
	  `no` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(255) DEFAULT NULL,
	  `cno` int(11) DEFAULT NULL,
	  PRIMARY KEY (`no`),
	  KEY `cno` (`cno`),
	  CONSTRAINT `t_student_ibfk_1` FOREIGN KEY (`cno`) REFERENCES `t_class` (`classno`)
	) ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8



>  在建表的时候可以在最后小括号的")"的右边使用：
>
> * ENGINE来指定存储引擎。
> * CHARSET来指定这张表的字符编码方式。
>
> 结论：mysql默认的存储引擎是：InnoDB；mysql默认的字符编码方式是：utf8



查看mysql版本：mysql> select version();

查看mysql支持哪些存储引擎：show engines \G



> MyISAM存储引擎特点：可被转换为压缩、只读表来节省空间
>
> InnoDB最大的特点就是支持事务：以保证数据的安全。效率不是很高，并且也不能压缩，不能转换为只读
>
> InnoDB缺点：不能很好的节省存储空间。
>
> MEMORY引擎优点：查询效率是最高的。不需要和硬盘交互。
> MEMORY引擎缺点：不安全，关机之后数据消失。因为数据和索引都是在内存当中。



# 七 事务（必须掌握）

一个事务其实就是一个完整的业务逻辑，是一个最小的工作单元。不可再分。

只有DML语句才会有事务这一说，其它语句和事务无关。
insert、delete、update是数据库表中数据进行增、删、改的。
**数据安全第一位**

事务transaction：就是批量的DML语句同时成功，或者同时失败



> 事务是怎么做到多条DML语句同时成功和同时失败的呢？
>
> * InnoDB存储引擎：提供一组用来记录事务性活动的日志文件
> * 在事务的执行过程中，每一条DML的操作都会记录到“事务性活动的日志文件”中。
> * 在事务的执行过程中，我们可以提交事务，也可以回滚事务。



## 7.1 实现事务

> 提交事务：commit; 语句
>
> * 清空事务性活动的日志文件，将数据全部彻底持久化到数据库表中。
> * 提交事务标志着，事务的结束。并且是一种全部成功的结束。
>
> 回滚事务：rollback; 语句（回滚永远都是只能回滚到上一次的提交点）
>
> * 将之前所有的DML操作全部撤销，并且清空事务性活动的日志文件
> * 回滚事务标志着，事务的结束。并且是一种全部失败的结束。



> mysql默认情况下是支持自动提交事务的。（自动提交）
> 自动提交：每执行一条DML语句，则提交一次！
>
> 这种自动提交实际上是不符合我们的开发习惯，因为一个业务通常是需要多条DML语句共同执行才能完成的，为了保证数据的安全，必须要求同时成功之后再提交，所以不能执行一条就提交一条。
>
> 将mysql的自动提交机制关闭掉：
>
> * 先执行这个命令：start transaction;
> * 提交commit; 
> * 回滚：rollback; 



## 7.2 事务特征

> * A：原子性
>   		说明事务是最小的工作单元。不可再分。
> * C：一致性
>   	所有事务要求，在同一个事务当中，所有操作必须同时成功，或者同时失败，以保证数据的一致性。
> * I：隔离性
>   	A事务和B事务之间具有一定的隔离。
> * D：持久性
>   	事务最终结束的一个保障。事务提交，就相当于将没有保存到硬盘上的数据保存到硬盘上



## 7.3 事务隔离级别

事务和事务之间的隔离级别有4个级别

| 级别                                                         | 英文             | 级别           | 含义                                                         | 存在的问题                                                   | 解决的问题               | 备注                                                         |
| ------------------------------------------------------------ | ---------------- | -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------ | ------------------------------------------------------------ |
| 读未提交《没有提交就读到了》                                 | read uncommitted | 最低的隔离级别 | 事务A可以读取到事务B未提交的数据。                           | 脏读现象！(Dirty Read)；读到了脏数据。                       |                          | 这种隔离级别一般都是理论上的，大多数的数据库隔离级别都是二档起步！ |
| 读已提交《提交之后才能读到》                                 | read committed   |                | 事务A只能读取到事务B提交之后的数据。                         | 不可重复读取数据。（在事务开启之后，第一次读到的数据是3条，当前事务还没有结束，可能第二次再读取的时候，读到的数据是4条，3不等于4称为不可重复读取） | 解决了脏读的现象。       | 这种隔离级别是比较真实的数据，每一次读到的数据是绝对的真实。<br/>oracle数据库默认的隔离级别是：read committed |
| 可重复读《提交之后也读不到，永远读取的都是刚开启事务时的数据》 | repeatable read  |                | 事务A开启之后，不管是多久，每一次在事务A中读取到的数据都是一致的。即使事务B将数据已经修改，并且提交了，事务A读取到的数据还是没有发生改变，这就是可重复读。 | 可以会出现幻影读。<br/>每一次读取到的数据都是幻象。不够真实！ | 解决了不可重复读取数据。 | mysql中默认的事务隔离级别就是这个                            |
| 序列化/串行化                                                | serializable     | 最高的隔离级别 |                                                              | 效率最低。这种隔离级别表示事务排队，不能并发！               | 解决了所有的问题。       | synchronized，线程同步（事务同步）<br/>每一次读取到的数据都是最真实的，并且效率是最低的。 |



## 7.4 验证各种隔离级别

 查看隔离级别：SELECT @@tx_isolation

验证：read uncommited
mysql> set global transaction isolation level read uncommitted;

验证：read commited
mysql> set global transaction isolation level read committed;

验证：repeatable read
mysql> set global transaction isolation level repeatable read;

验证：serializable
mysql> set global transaction isolation level serializable;

设置完后要退出，再验证

# 八 索引（index）

索引是在数据库表的字段上添加的，是为了提高查询效率存在的一种机制。
一张表的一个字段可以添加一个索引，当然，多个字段联合起来也可以添加索引。
索引，是为了缩小扫描范围而存在的一种机制。

MySQL在查询方面主要就是两种方式：

* 全表扫描
* 根据索引检索

在mysql数据库当中索引也是需要排序的，并且这个排序和TreeSet数据结构相同。TreeSet（TreeMap）底层是一个自平衡的二叉树！在mysql当中索引是一个B-Tree数据结构。

遵循左小又大原则存放。采用中序遍历方式遍历取数据。



## 8.1 索引的实现原理

在任何数据库当中主键上都会自动添加索引对象；另外在mysql当中，一个字段上如果有unique约束的话，也会自动创建索引对象。

在任何数据库当中，任何一张表的任何一条记录在硬盘存储上都有一个硬盘的物理存储编号。

在mysql当中，索引是一个单独的对象，不同的存储引擎以不同的形式存在，在MyISAM存储引擎中，索引存储在一个.MYI文件中。在InnoDB存储引擎中索引存储在一个逻辑名称叫做tablespace的当中。在MEMORY存储引擎当中索引被存储在内存当中。不管索引存储在哪里，索引在mysql当中都是一个树的形式存在。（自平衡二叉树：B-Tree）

![](图表\索引的实现原理.png)

**在mysql当中，主键上，以及unique字段上都会自动添加索引的**

建议不要随意添加索引，因为索引也是需要维护的，太多的话反而会降低系统的性能。
建议通过主键查询，建议通过unique约束的字段进行查询，效率是比较高的。

考虑给字段添加索引：

* 数据量庞大
* 该字段经常出现在where的后面，以条件的形式存在，也就是说这个字段总是被扫描
* 该字段很少的DML(insert delete update)操作。（因为DML之后，索引需要重新排序。）



## 8.2 修改删除索引

创建索引：

	mysql> create index emp_ename_index on emp(ename);
	给emp表的ename字段添加索引，起名：emp_ename_index

删除索引：

	mysql> drop index emp_ename_index on emp;
	将emp表上的emp_ename_index索引对象删除。

查看一个SQL语句是否使用了索引进行检索

	mysql> explain select * from emp where ename = 'KING';



## 8.3  索引的失效

| 情况                                               | 例子                                                         | 原因                                                         |
| -------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 模糊匹配当中以“%”开头了                            | select * from emp where ename like '%T';                     | ename上即使添加了索引，也不会走索引，原因是因为**模糊匹配当中以“%”开头了！尽量避免模糊查询的时候以“%”开始。**这是一种优化的手段/策略 |
| 使用or的时候会失效                                 |                                                              | 如果使用or那么要求or两边的条件字段都要有索引，才会走索引，如果其中一边有一个字段没有索引，那么另一个字段上的索引也会实现。所以这就是为什么不建议使用or的原因。 |
| 使用复合索引的时候，没有使用左侧的列查找，索引失效 |                                                              | 两个字段，或者更多的字段联合起来添加一个索引，叫做复合索引。 |
| 在where当中索引列参加了运算，索引失效。            | mysql> create index emp_sal_index on emp(sal);<br/>explain select * from emp where sal = 800; |                                                              |
| 在where当中索引列使用了函数                        |                                                              |                                                              |

**索引是各种数据库进行优化的重要手段。优化的时候优先考虑的因素就是索引。**
索引在数据库当中分了很多类？

* 单一索引：一个字段上添加索引。
* 复合索引：两个字段或者更多的字段上添加索引。



* 主键索引：主键上添加索引。
* 唯一性索引：具有unique约束的字段上添加索引。
  .....

注意：唯一性比较弱的字段上添加索引用处不大。



# 九 视图

view:站在不同的角度去看待同一份数据。

创建视图对象：

	create view dept2_view as select * from dept2;

删除视图对象：

	drop view dept2_view;

只有DQL语句才能以view的形式创建。
create view view_name as 这里的语句必须是DQL语句;



可以面向视图对象进行增删改查，对视图对象的增删改查，会导致原表被操作！（视图的特点：通过对视图的操作，会影响到原表数据。）

		//面向视图查询
		select * from dept2_view; 
	
		// 面向视图插入
		insert into dept2_view(deptno,dname,loc) values(60,'SALES', 'BEIJING');
	
		// 查询原表数据
		mysql> select * from dept2;
	
		// 面向视图删除
		mysql> delete from dept2_view;
	
		// 查询原表数据
		mysql> select * from dept2;
	
		// 创建视图对象
		create view 
			emp_dept_view
		as
			select 
				e.ename,e.sal,d.dname
			from
				emp e
			join
				dept d
			on
				e.deptno = d.deptno;
	
		// 查询视图对象
		mysql> select * from emp_dept_view;
		// 面向视图更新
		update emp_dept_view set sal = 1000 where dname = 'ACCOUNTING';
	
		// 原表数据被更新
		mysql> select * from emp;



视图对象在实际开发中的作用：方便，简化开发，利于维护

面向视图开发的时候，使用视图的时候可以像使用table一样。可以对视图进行增删改查等操作。视图不是在内存当中，视图对象也是存储在硬盘上的，不会消失。



视图对应的语句只能是DQL语句。但是视图对象创建完成之后，可以对视图进行增删改查等操作。



增删改查，又叫做：CRUD。CRUD是在公司中程序员之间沟通的术语。一般我们很少说增删改查。一般都说CRUD。

		C:Create（增）
		R:Retrive（查：检索）
		U:Update（改）
		D:Delete（删）



# 十 DBA命令

重点掌握：数据的导入和导出（数据的备份）

> 数据导出？
>
> 		在windows的dos命令窗口中：
> 				mysqldump bjpowernode>D:\bjpowernode.sql -uroot -p123456
> 		可以导出指定的表吗？
> 			mysqldump bjpowernode emp>D:\bjpowernode.sql -uroot -p123456



> 数据导入
> 		需要先登录到mysql数据库服务器上。
> 		然后创建数据库：create database bjpowernode;
> 		使用数据库：use bjpowernode
> 		然后初始化数据库：source D:\bjpowernode.sql

# 十一 数据库设计三范式（面试官经常问的）

数据库设计范式：数据库表的设计依据。教你怎么进行数据库表的设计。

| 范式     | 含义                                                         | 备注                                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 第一范式 | 要求任何一张表必须有主键，每一个字段原子性不可再分           | 最核心，最重要的范式，所有表的设计都需要满足。<br/>必须有主键，并且每一个字段都是原子性不可再分。 |
| 第二范式 | 建立在第一范式的基础之上，要求所有非主键字段完全依赖主键，不要产生部分依赖 | 建立在第一范式的基础之上，<br/>要求所有非主键字段必须完全依赖主键，不要产生部分依赖 |
| 第三范式 | 建立在第二范式的基础之上，要求所有非主键字段直接依赖主键，不要产生传递依赖。 | 第三范式建立在第二范式的基础之上<br/>要求所有非主键字典必须直接依赖主键，不要产生传递依赖。 |

设计数据库表的时候，按照以上的范式进行，可以避免表中数据的冗余，空间的浪费。

数据库设计三范式是理论上的。

实践和理论有的时候有偏差。

最终的目的都是为了满足客户的需求，有的时候会拿冗余换执行速度。

因为在sql当中，表和表之间连接次数越多，效率越低。（笛卡尔积）

有的时候可能会存在冗余，但是为了减少表的连接次数，这样做也是合理的，
并且对于开发人员来说，sql语句的编写难度也会降低。



## 总结表的设计

一对多：一对多，两张表，多的表加外键

多对多：多对多，三张表，关系表两个外键

一对一：在实际的开发中，可能存在一张表字段太多，太庞大。这个时候要拆分表。
一对一设计：没有拆分表之前：一张表

口诀：一对一，外键唯一
