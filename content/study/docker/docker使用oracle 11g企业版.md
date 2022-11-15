+++
title = "Docker安装Oracle 11g EE"
chapter =  true
+++

# Docker使用Oracle11g企业版

拉取 [oracle-ee-11g](https://registry.hub.docker.com/r/loliconneko/oracle-ee-11g) 镜像

```shell
docker pull loliconneko/oracle-ee-11g
```

运行容器

```shell
docker run -d --name oracle_ee_11g -p 1521:1521 loliconneko/oracle-ee-11g 
```

```shell
hostname: localhost
port: 1521
sid: EE
service name: EE.oracle.docker
username: system
password: oracle
```

进入容器同步时间

```shell
#进入容器
docker exec -it oracle11g bash
#同步时间
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

修改字符集为GBK

```shell
sqlplus sys/oracle as sysdba
 
SQL> shutdown immediate;
SQL> startup mount;
SQL> alter system enable restricted session;
SQL> alter system set job_queue_processes=0;
SQL> alter system set aq_tm_processes=0;
SQL> alter database open;
SQL> ALTER DATABASE character set INTERNAL_USE ZHS16GBK;
SQL> shutdown immediate;
SQL> startup; 
```

修改服务名为orcl

```shell
#修改 global_name
SQL> alter database rename global_name to orcl;
SQL> update global_name set global_name='orcl';
SQL> commit;
#修改 db_domain
SQL> alter system set db_domain='' scope=spfile;
#修改 service_name
SQL> alter system set service_names='oracle'; 
#重启
SQL> shutdown immediate
SQL> startup; 
```

导出容器为镜像

```shell
docker commit  oracle_ee_11g  oracle_ee_11g_gbk
```

完成！
