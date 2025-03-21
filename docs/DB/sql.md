# SQL 
structured query language (结构化查询语言)

## data definition

### domain types 
char(n):长度是n,注意不同于C语言的是没有'\0'结尾

varchar(n):不定长字符串，最长是n

int

smallint

numeric(p,d):p是有效数字个数，d是小数点后几位。

real,double precision

float(n)

### built-in data
- date : year-month-day,eg:data '2005-6-12'
- time : eg:time '09:00:30'
- timetamp:data+time
- interval:
  
    - eg: interval '1' day


## creat table construct

创立,以及主键和外键：

```sql
create table instructor(
    ID char(5),
    name varchar(20) not null
    dept_name varchar(10),
    primary key(ID),
    foreign key(dept_name) references department
)
```

delete cascade
update cascade
set null

- drop:删表
- delete：删内容不删表（即保留关系模式）
- alter table r add A D：更改表的定义，A是增加的属性，D是数据类型。如：alter table student add resume varchar(256)

## select
select A1,A2,...An from r1,r2,..rn,where p 
- from 后面就算几个表乘起来

自然连接的等价：如：

```sql
--> 假设r1,r2的公共属性为b
select a1,a2
from r1,r2
where r1.b=r2.b
--> 等价于：
select a1,a2
from r1 natural join r2

```

select distinct ...
select * from ...

## rename
as

## 字符串操作
`%` :表示任意长度的字符串，例如：

```sql
select name from instructor where name like %dar%
```

`_` :  表示单个字符，如` _ _ _ `表示长度为3的字符串

## order排序
order by ... desc/asc

limit 限制个数

## 集合操作
union ,intersect,except(并，交，减) 

## aggregate functions

首先选出instructor表中薪水大于100的，然后按照dept_name分组，然后选择人数超过10的留下来，最后按照人数排序输出（cnt）
```sql
select dept_name,count(*) as cnt
from instructor
where salary>100
group by dept_name
having count(*)>10
order by cnt
```

## nested subqueries(重点)
- in,not in
- exist,not exist
- unique