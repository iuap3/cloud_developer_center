# 智能搜索服务

## 数据检索帮助文档--根据类SQL语法查询数据 ##


## 目前opeansearch支持的分词类型

- 不分词

- 模糊分词

- 拼音分词

- IK分词



## 查询对比

### 说明

- 如果字段都是精确查询，索引字段请选择不分词。

- 如果字段需要模糊查询，但是在某些场景下需要精确查询，查询的时候，请使用`字段名.raw`作为条件。

- 对于分词的字段进行聚合，如`group by,sort`等操作的时候，请使用`字段名.raw`作为聚合条件



### EQ 查询

#### 标准sql查询

```sql

select * from user where name='数据检索'

```

#### 不分词

```sql

select * from user where name='数据检索'

```



#### 模糊分词

```sql

select * from user where name=term('数据检索')

```



#### 拼音分词

```sql

select * from user where name=term('shujujiansuo')

select * from user where name=term('sjjs')

```

#### IK分词

```sql

select * from user where name=matchquery('数据检索')

```



### like 查询

#### 标准sql查询

```sql

select * from user where name like '数据%'

```

#### 不分词

```sql

select * from user where name like '数据%'

```



#### 模糊分词

```sql

select * from user where name=term('数据')

```



#### 拼音分词

```sql

select * from user where name=term('shuju')

select * from user where name=term('sj')

```

#### IK分词

```sql

select * from user where name=matchquery('数据')

```



## 不等于

```sql

select * from user where age <> 20

select * from user where age is not 20

```



## GT,GTE,LT,LTE

```sql

select * from user where age >= 20 and age <= 30

```



## between and

```sql

select * from user where age between 20 and 30

```



## in

```sql

select * from user where age in(20,30)

```



## is null / is not null

```sql

select * from user where name is null

```





## 聚合

```sql

select * from user group by dept

```



## min,max,avg,sum,count

```sql

select avg(age) from user 

```

## stats

```sql

select stats(age) from user 

```



## percentiles 百分比

```sql

select percentiles(age) from user 

```



#### 根据区间聚合

```sql

 select count(age) from bank group by range(age, 20,25,30,35,40)

```



#### 根据时间区间聚合

```sql

select online from online group by date_histogram(field='insert_time','interval'='1d')

```
