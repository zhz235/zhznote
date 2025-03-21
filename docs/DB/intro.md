# 数据库介绍

## database system
Database system = Database management system(DBMS) + App. + Users

目前可以把数据库理解为包含关系的表格

## purpose of database system

如果应用建立在file systems上，会有以下几个问题：

- 数据冗余与不一致（data redundancy and inconsistency）:同一数据可能存在很多不同的地方；由于数据彼此不互通，还可能带来有的有的修改有的没修改的问题，带来不一致
- 数据孤立(data isolation):不同数据不互通
- 存取数据困难(difficulty in accessing data)：对每个任务都要写新的程序
- integrity problems（完整性问题）
- atomicity problems（原子性问题）
- concurrent access anomalies(并发异常访问)
- security problems

而database system可以解决这些问题

## view of data
三层抽象：

- physical level:最底层抽象，描述数据如何保存（how）
- logical level:数据库存储什么数据，数据之间的关系？（what）
- view level:不同人看到他们所需要的数据

设计三层抽象的目的是隐藏复杂性；同时增强应用适应变化的能力，不会因为一个新需求就要改变整个应用程序。

当然，三层抽象之间也通过映射来联系，及三层结构两层映射。

### schema(模式) and instance(实例)
可以理解成C语言中类型与变量的关系

- Schema： the logical structure of the database (physical/logical)
- Instance： the actual content of the database at a particular point in time

### data independence
- Physical Data Independence (物理数据独立性): the ability to modify the physical schema without changing the logical schema
- Logical Data Independence (逻辑数据独立性): the ability to modify the logical schema without changing the user view schema

## data models(数据模型)
a collection of tools for describing:

- data
- data relationships
- data semantics(语义)
- data constraints(约束)

常见模型：

- relational model
- entity-relationship data model(实体-联系模型)
- object-based data model
    - object-oriented（面向对象数据模型）
    - object-relational（对象-关系模型）
- semistructured data model（半结构化数据模型）

## database languages
### DDL（数据定义语言）
data dictionary contains **metadata**(元数据)，元数据是用来描述数据的数据

### DML(数据操作语言)
query language

### SQL(结构化查询语言)