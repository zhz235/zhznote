# Relational Model 

## concepts
定义：一个关系指的是元组的集合

- 无序
- 无重复

假设$A_1,A_2....A_n$是attribute，R=($A_1,A_2....A_n$)称为relation schema。例如：instructor=（ID,name,salary）

A relation instance r defined over schema R is denoted by r(R).

database schema是抽象的实现，database instance 是具体的实例，是某一时刻数据库中数据的快照（snapshot）

### attributes
- the set of allowed values for each attribute is called the domain of the attribute
- null is a member of every domain
- attribute values is atomic,indivisible

## keys 
- superkey:能唯一确定元组的属性
- candidate key：如果超键不可再分，则称为候选键
- primary key:由候选键中选择出
- foreign key（外键）:关系r1中的某个属性必须是r2中对应属性的主键。类似指针，即r1的外键指向的是r2中的主键。注意，** foreign key指向null 或 别的表的主键**
    - 外键约束（foreign key constraint）:有益于维护数据完整性
    - referential integrity（参照完整性）：类似于外键限制，但不局限于主键

## relational algebra
### select: $\sigma$
### project: $\Pi$
 类似投影，取某些属性并忽略其他属性，会对最后结果去重
### union: U
### set difference: -
### cartesian product: X
### rename :$\rho$
### 交：r∩s=r-(r-s)
### natural-join operation: $\Join$
相当于先做笛卡尔积，再选择，最后投影

具体来说，先找到相同属性一样的，进行拼接，然后投影出所有属性

条件连接theta join：$r \Join _\theta s=\sigma _\theta (r*s)$