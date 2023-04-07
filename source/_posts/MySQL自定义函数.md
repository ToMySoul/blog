---
title: Mysql函数
date: 2023-04-07 11:26:07
categories: 数据库
tags:
- 函数
- Mysql
---

### 创建表格
```
-- 创建表格
CREATE TABLE souk(
id BIGINT  not null  COMMENT'主键',
`name` VARCHAR(64) COMMENT'用户名',
`register` char(12) DEFAULT("system") COMMENT'注册方式',
create_time TIMESTAMP DEFAULT(CURRENT_TIMESTAMP) COMMENT'创建时间',
PRIMARY KEY(id)

)
```

###  批量插入数据（循环）
```
-- 生成数据

SET GLOBAL log_bin_trust_function_creators=TRUE; -- 创建函数一定要写这个
DELIMITER $$   -- 写函数之前必须要写，该标志

CREATE FUNCTION mock_data()		-- 创建函数（方法）
RETURNS INT 						-- 返回类型
BEGIN								-- 函数方法体开始
	DECLARE num INT DEFAULT 1000000; 		-- 定义一个变量num为int类型。默认值为100 0000
	DECLARE i INT DEFAULT 0; 
	
	WHILE i < num DO 				-- 循环条件
		 INSERT INTO souk(`name`,`register`) 
		 VALUES(CONCAT('用户',i),'systemn');
		SET i =  i + 1;	-- i自增	
	END WHILE;		-- 循环结束
	RETURN i;
END; 								-- 函数方法体结束


```
---
### 简单的存储过程

创建表
```

#准备工作
CREATE TABLE admin (
          id INT PRIMARY KEY AUTO_INCREMENT,
          user_name VARCHAR (15) NOT NULL,
          pwd VARCHAR (25) NOT NULL
) ;

 CREATE TABLE beauty (
          id INT PRIMARY KEY AUTO_INCREMENT,
          NAME VARCHAR (15) NOT NULL,
          phone VARCHAR (15) UNIQUE,
          birth DATE
) ;

INSERT INTO beauty(NAME,phone,birth) VALUES 
('朱茵','13201233453','1982-02-12'), 
('孙燕姿','13501233653','1980-12-09'), 
('田馥甄','13651238755','1983-08-21'), 
('邓紫棋','17843283452','1991-11-12'), 
('刘若英','18635575464','1989-05-18'), 
('杨超越','13761238755','1994-05-11'); 

```

通过id获取姓名，手机号
```
CREATE DEFINER=`root`@`localhost` PROCEDURE `get_phone`(IN id INT,OUT bname VARCHAR(15),OUT bphone VARCHAR(15))
BEGIN
	SELECT beauty.name , beauty.phone 
	       INTO bname,bphone
	FROM beauty WHERE beauty.id = id;
END
```
调用
```
CALL get_phone(1,@name,@phone);
SELECT @name,@phone;
```

创建存储过程insert_user(),实现传入用户名和密码，插入到admin表中 
```
DELIMITER $  #‘$’代表着语句的结束
CREATE PROCEDURE insert_user(IN username VARCHAR(15),IN `password` VARCHAR(25))
BEGIN 
	INSERT INTO admin VALUES (null,username,`password`);
END $  #存储过程结束
DELIMITER ;  #修改语句结束的标识符为‘;’
```
调用
```
CALL insert_user("125214521212521","1212125214252125212512521");
```
