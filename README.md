## 练习1 

1. 创建数据库employee_db
2. 在数据库employee_db中创建table：`Employee`
3. 将`employee-data.csv`文件中的数据导入数据表Employee中
4. 在数据库employee_db中创建table：`Company`
5. 将`employee-data.csv`文件中的数据导入Company数据表中
6. 找出Employee表中姓名包含`n`字符并且薪资大于6000的雇员所有个人信息

### 前5小问解答

```
$ mysql -u root -p  --local-infile=1
```

``` mysql
CREATE DATABASE IF NOT EXISTS `employee_db` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
use employee_db;
```

``` mysql
create table Employee(
  id int primary key, 
  name varchar(20) not null, 
  age int not null, 
  gender varchar(20) not null, 
  companyId int not null,
  salary int not null
) engine=InnoDB DEFAULT CHARSET = utf8;
```

``` mysql
LOAD DATA LOCAL INFILE '/Users/shiqi/Downloads/MySQL-Quiz-2019-1-8-3-26-0-483/employee-data.csv' INTO TABLE Employee FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' (id, name, age, gender, companyId, salary);
```

``` mysql
create table Company(
  id int primary key, 
  companyName varchar(20) not null, 
  number int not null
) engine=InnoDB DEFAULT CHARSET = utf8;
```

``` mysql
LOAD DATA LOCAL INFILE '/Users/shiqi/Downloads/MySQL-Quiz-2019-1-8-3-26-0-483/company-data.csv' INTO TABLE Company FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' (id, companyName, number);
```

### 第6小问解答

``` mysql
SELECT * FROM Employee WHERE `name` LIKE '%n%' AND salary > 6000;
```

### 运行结果截图：

![](https://ws1.sinaimg.cn/large/006tNc79ly1g251w5ouu9j30ij0c4mzk.jpg)

只有一个人符合条件

## 练习2 

取得每个公司中最高薪水的人员名字

输出结果包含公司名称和人员名称：companyName name

### 解答

``` mysql
select 
  companyName, name 
from 
  Company left join Employee
on 
  Employee.companyId = Company.id 
and 
  Employee.salary = (select max(Salary) from Employee where CompanyId=Company.Id);
```

### 运行结果截图：

![](https://ws3.sinaimg.cn/large/006tNc79ly1g251wm6xtzj30ij0c3acy.jpg)

## 练习3 

取得平均薪水最高的公司

输出公司名称和平均薪水：companyName avgSalary

### 解答
``` mysql
select 
  companyName, avgSalary 
from 
  Company left join (select companyId, avg(salary) as avgSalary from Employee Company GROUP BY companyId) T
on 
  Company.id = T.companyId 
order by 
  avgSalary DESC limit 1;
```

### 运行结果截图：

![](https://ws1.sinaimg.cn/large/006tNc79ly1g251wrcwxuj30ig0c5q4r.jpg)

平均薪水最高的是 tengxun 公司

## 练习4 

找出薪水在公司的平均薪水之上的人员名字

输出标准中包含Employee中所有字段和该employee所在公司名称以及该公司平均薪资：

> id | name | age | gender | companyId | salary | companyName | avgsal


### 解答：

``` mysql
select Employee.*, B.companyName, B.avgsal
from 
  Employee,
(select 
  companyName, companyId, avgsal 
from 
  Company left join (select companyId, avg(salary) as avgsal from Employee Company GROUP BY companyId) T
on 
  Company.id = T.companyId) B
where
  Employee.companyId = B.companyId and Employee.salary >= B.avgsal;
```

### 运行结果截图：

![](https://ws3.sinaimg.cn/large/006tNc79ly1g251wym377j30j20c2dip.jpg)

3个公司各有一名员工的工资，大于等于该公司平均工资。可以看到，这个结果与练习2中各公司的薪水最高的人结果一致。


---

# MySQL基础
## 练习描述

本题要求使用 `MySQL` 来完成数据库数据表的创建，并使用查询语句完成基本的查询操作。
- 公司 `Company` 的字段有：`id`,`companyName`,`employeesNumber`
- 员工 `Employee` 的字段有：`id`,`name`,`age`,`gender`,`companyId`,`salary`
- 按照`practice1-practice4`的需求完成数据的查询

要求：
- 创建新的文件`result.txt`
- 将每个practice的查询结果都粘贴至`result.txt`文件中，例如：

```
 practice1
 +----+------------+-----+----------+-----------+--------+
 | id | name       | age | gender   | companyId | salary |
 +----+------------+-----+----------+-----------+--------+
 |  1 | '********' |  19 | 'female' |         1 |   7000 |
 +----+------------+-----+----------+-----------+--------+
 
 practice2
 +----+------------+-----+----------+
 | id | companyName| employeesNumber| 
 +----+------------+-----+----------+
 |  1 | 'baidu'    |     1000       |   
 +----+------------+-----+----------+  
```

## 环境要求
- MySQL:5.7

## 如何开始
- 本地启动`MySQL`
- 按照`practice1.sql`里的需求描述，完成数据库、数据表的搭建


## 输出规范
- 完成`practice1-practice4`文件中的所有需求
