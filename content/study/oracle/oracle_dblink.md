+++
title = "Oracle_dblink"
chaper = true
+++

# 推荐使用Datax等

## 1、dblink创建

``` sql
-- 合并
create public database link HIS connect to 对方数据库帐号 identified by 对方数据库密码  using '(DESCRIPTION = (ADDRESS = (PROTOCOL = TCP)(HOST = 本机ipv4地址)(PORT = 1521)) (CONNECT_DATA =  (SERVER = DEDICATED) (SERVICE_NAME = orcl)))';

-- 不合并
create public database link HIS
 connect to "对方数据库帐号"
  identified by "对方数据库密码"
  using 'tnsnames.ora中添加的名字';
  
```

## 2、修改Listener后重启监听(设置dblink必须重启否则不生效)

```
计算机->管理->服务，找到oracle的自启服务，找到OracleOraDb11g_home1TNSListener
```