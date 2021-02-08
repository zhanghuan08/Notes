# Mysql操作

## 基本命令

**1、新建用户：**

```mysql
CREATE USER name IDENTIFIED BY 'ssapdrow';
```

**2、更改密码：**

```mysql
SET PASSWORD FOR name=PASSWORD('fdddfd');
```

**3、权限管理**

```mysql
SHOW GRANTS FOR name; //查看name用户权限
GRANT SELECT ON db_name.* TO name;　　　　//给name用户db_name数据库的所有权限
REVOKE SELECT ON db_name.* TO name;　　　　//GRANT的反操作，去除权限；
```

##一、数据库操作：　

**1、查看数据库：**

```mysql
SHOW DATABASES;
```

**2、创建数据库：**

```mysql
CREATE DATABASE db_name;　　//db_name为数据库名
```

**3、使用数据库：**

```mysql
USE db_name;
```

**4、删除数据库：**

```mysql
DROP DATABASE db_name;
```

**5、查看数据库中可用的表：**

```mysql
SHOW TABLES;
```

**6 、查看表的结构**

```mysql
describe 表名; #可以简写为desc 表名;
```

### 1.1、插入数据：

**1、插入数据：**

```mysql
INSERT INTO tb_name(id,name,score)VALUES(NULL,'张三',140),(NULL,'张四',178),(NULL,'张五',134);
```

**2、插入检索出来的数据：**

```mysql
INSERT INTO tb_name(name,score) SELECT name,score FROM tb_name2;
```

### 1.2、删除数据：

**1、删除数据：**先判断表是否存在,存在先删除

```mysql
DELETE FROM tb_name WHERE id=3;
```

### 1.3、条件控制：

**1、WHERE 语句：**

```mysql
SELECT * FROM tb_name WHERE id=3;
```

**2、HAVING 语句：**

```mysql
SELECT * FROM tb_name GROUP BY score HAVING count(*)>2
```

**3、相关条件控制符：** 

>　=、>、<、<>、IN(1,2,3......)、BETWEEN a AND b、NO、AND 、OR
>
>　Linke()用法中 % 为匹配任意、 _ 匹配一个字符（可以是汉字）
>
>　IS NULL 空值检测

## 二、Mysql分页查询

**1、limit分页公式：**curPage是当前第几页；pageSize是一页多少条记录

limit (curPage-1)*pageSize,pageSize

**2、用的地方：sql语句中**

```mysql
select * from student limit(curPage-1)*pageSize,pageSize;
```

### 总页数公式

1、总页数公式：totalRecord是总记录数；pageSize是一页分多少条记录

​			int totalPageNum = (totalRecord +pageSize - 1) / pageSize;

2、用的地方：前台UI分页插件显示分页码

3、查询总条数：totalRecord是总记录数

```sql
SELECT COUNT(*) FROM tablename
```

### 创建分页类

```java
package com.hopu.domain;

import java.util.List;

public class PageBean<T> {
    //总记录数
    private Integer totalRecord;
    //总页数
    private Integer totalPage;
    //每页显示的记录数
    private Integer pageSize;
    //当前页
    private Integer pageNum;
    //将每页要显示的数据放在list集合中
    private List<T> list;

    public PageBean(Integer totalRecord, Integer pageSize, Integer pageNum) {
        this.totalRecord=totalRecord;
        this.pageSize=pageSize;
        this.pageNum=pageNum;

        //总页数 totalPage
        if (totalRecord/pageSize==0){
            //说明整除，正好每页显示pageSize条数据，没有多余一页要显示少于pageSize条数据的
            this.totalPage = totalRecord / pageSize;
        }else {
            //不整除，就要在加一页，来显示多余的数据。
            this.totalPage = totalRecord / pageSize +1;
        }
    }

    public Integer getTotalRecord() {
        return totalRecord;
    }

    public void setTotalRecord(Integer totalRecord) {
        this.totalRecord=totalRecord;
    }

    public Integer getTotalPage() {
        return totalPage;
    }

    public void setTotalPage(Integer totalPage) {
        this.totalPage=totalPage;
    }

    public Integer getPageSize() {
        return pageSize;
    }

    public void setPageSize(Integer pageSize) {
        this.pageSize=pageSize;
    }

    public Integer getPageNum() {
        return pageNum;
    }

    public void setPageNum(Integer pageNum) {
        this.pageNum=pageNum;
    }

    public List<T> getList() {
        return list;
    }

    public void setList(List<T> list) {
        this.list=list;
    }
}
```

