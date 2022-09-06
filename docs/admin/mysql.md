# MySQL数据库管理
## 创建管理员
```SQL
create user 'username'@'%' identified by 'password';
grant all privileges on *.* to 'username'@'%';
```
## 修改某个用户密码
```SQL
alter user 'username'@'%' identified by 'newpassword';
```
## 管理员批量创建用户脚本
```SQL
-- 这个脚本能批量创建用户，用户名为st+学号，对应一个学生自己的同名数据库有所有权限，对于其他数据库有只读权限
use mysql;
set global validate_password_policy=0;
set global validate_password_length=1;
drop table if exists stu_list;
create table stu_list(
	id char(7) PRIMARY KEY,
    pwd varchar(20)
);
-- 这里输入('学号', '密码') 可以学生自己修改，但至少要有一个字符
insert into stu_list values
('1810063', 'alex'),
('1810064', 'bobb');

-- 批量的存储过程
drop procedure if exists create_stu;
delimiter ;;
create DEFINER=`root`@`localhost` procedure create_stu()
begin
    declare ids char(7);
    declare pwds varchar(20);
	declare done int default false;
    declare cur cursor for select `id` from `stu_list`;
	declare continue handler for NOT FOUND set done = true;
	open cur; -- 开始游标
	LOOP_LABLE:loop
		FETCH cur INTO ids;
		select pwd into pwds from stu_list where id=ids;
        set @name = CONCAT('st', ids);
        set @create_db_sql = concat('create database if not exists ', @name, ';');
		PREPARE stmt FROM @create_db_sql;
		EXECUTE stmt;
        SET @user_sql = CONCAT('GRANT ALL PRIVILEGES ON ', @name,".* TO '", @name, "'@'%' IDENTIFIED BY '", pwds, "';");  
        PREPARE stmt FROM @user_sql;
        EXECUTE stmt;
        SET @user_sql = CONCAT('GRANT SELECT ON ', "*.* TO '", @name, "'@'%';");
		PREPARE stmt FROM @user_sql;
        EXECUTE stmt;
        FLUSH PRIVILEGES;
		if done THEN
			LEAVE LOOP_LABLE;
		END IF;
		end LOOP;
	CLOSE cur;
	FLUSH PRIVILEGES;
end; 
;;
delimiter ;

-- 调用
CALL create_stu();

drop database if exists st1810063;
drop database if exists st1810064;
drop user if exists st1810063;
drop user if exists st1810064;
drop table stu_list;
```