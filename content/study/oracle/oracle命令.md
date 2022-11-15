+++
title = "Oracle命令"
chaper = true
+++

## 1、启停Oracle

```sql
----暂停
shutdown immediate;
----启动
startup;
```

## 2、删除

### 2.1、删除表空间

```sql
DROP TABLESPACE tbs_xxx INCLUDING CONTENTS AND DATAFILES;
```

### 2.2删除用户

```sql
drop user xxx cascade;
```

### 2.3删除数据文件

```sql
alter database datafile 'E:\xxx\xxx.DBF ’ offline drop;
```

### 2.4删除表数据

```sql
TRUNCATE TABLE 表名；
```

###  2.5删除序列

```sql
DROP SEQUENCE seq_sys_menu;
```

## 3、修改

### 3.1、修改字符集

```sql
--查询字符集
Select userenv('language') From dual;
--修改字符集
sqlplus sys/sys as sysdba;
shutdown immediate;
startup mount;
alter system enable restricted session;
alter system set job_queue_processes=0;
alter system set aq_tm_processes=0;
alter database open;
alter database character set internal_use ZHS16GBK;
shutdown immediate;
startup;
```

### 3.2、修改序列值

```sql
select max(id) from CA_RBRVS_SCORE_DETAIL;
SELECT SEQ_CA_RBRVS_SCORE_DETAIL.NEXTVAL FROM DUAL;
ALTER SEQUENCE SEQ_CA_RBRVS_SCORE_DETAIL INCREMENT BY 351;
SELECT SEQ_CA_RBRVS_SCORE_DETAIL.NEXTVAL FROM DUAL;
ALTER SEQUENCE SEQ_CA_RBRVS_SCORE_DETAIL INCREMENT BY 1;
```

## 4、创建

### 4.1、创建表空间

```sql
create tablespace tbs_xxx datafile '/home/xxx.dbf' size 1024m AUTOEXTEND ON NEXT 50M MAXSIZE UNLIMITED;
```

### 4.2、创建用户

```sql
create user xxx identified by xxx default tablespace TBS_xxx quota unlimited on tbs_xxx;
grant connect to xxx;
grant resource to xxx;
grant select on v_$statname to xxx;
grant select on v_$sesstat to xxx;
grant select on v_$session to xxx;
grant select on v_$mystat to xxx;
-- 创建视图权限
GRANT CREATE VIEW TO xxx;
-- 创建函数权限
GRANT CREATE ANY PROCEDURE TO xxx;
-- 创建dblink权限
grant create public database link,create database link to xxx;
-- 数据泵权限
grant read,write on directory dump_file to xxx;
```

### 4.3、创建序列

```sql
create sequence seq_${tableName}
increment by 1
start with 10
nomaxvalue
nominvalue
cache 20;
```

## 5、数据泵导入导出（解决字符集问题）

```shell
----创建expdp默认文件路径（dump_file为自定义的变量名）
create directory dump_file as 'D:\EXPDP';

----导出命令
expdp xxx/xxx directory=dump_file dumpfile=xxx.dmp
----导入命令
impdp xxx/xxx directory=dump_file dumpfile=xxx.dmp full=y
```

## 6、随机数

```sql
TRUNC(1000+2000*dbms_random.value)
```

## 7、更新一张表数据为另一张表

``` sql
update table1 set table1.val = (select val from table2 where table1.idd = table2.idd);
table2中无时不做更新
update table1 set val = (select val from table2 where table1.idd = table2.idd) where exists (select 1 from table2 where table1.idd = table2.idd)；
```

## 8、时间查询

```sql
select * from Table_name where Field_name between to_date('2003-12-19 15:00:00','yyyy-mm-dd hh24:mi:ss') and to_date('2003-12-19 16:00:00','yyyy-mm-dd hh24:mi:ss')
```


## 9、oracle修改密码(Sessiong过期)

```sql
----修改密码(如果已经过期，则需要修改密码后再设置为永不过期，新密码可以与原来密码相同)
alter user  xxx identified by xxx;
---查询过期时间
SELECT * FROM dba_profiles s WHERE s.profile='DEFAULT' AND resource_name='PASSWORD_LIFE_TIME';
---修改为永不过期
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
```