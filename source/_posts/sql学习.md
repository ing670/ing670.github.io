---
title: sql学习
date: 2017-12-27 17:16:47
tags:
- sql
---
### 常用sql
```sql
#创建库
CREATE SCHEMA `库` DEFAULT CHARACTER SET utf8 ; 
#删除库
DROP TABLE `test`.`user`;
#插入数据
INSERT INTO `库`.`表` (`字段`) VALUES ('值'); 
#修改表，新增表字段
ALTER TABLE `test`.`user` 
ADD COLUMN `name` VARCHAR(45) NULL COMMENT '用户名' AFTER `id`,
ADD COLUMN `password` VARCHAR(45) NULL COMMENT '用户密码' AFTER `name`; 
#修改表，修改字段，尽量不要乱改字段
ALTER TABLE `test`.`user` 
CHANGE COLUMN `password` `pass` VARCHAR(45) NULL DEFAULT NULL COMMENT '用户密码' ;
```
### 如果开发的时候新增了字段怎么办
使用工具对比新库和旧的库，把新增的sql，重新导入进去