+++
title = "Dos命令"
chapter =  true
+++

# Dos命令
## 1.dos注释

``` basic
rem 注释
```

## 2.多条合并成一条执行

```basic
cd d:/ && cd d:/
```

## 3.获取管理员权限

```basic
%1 %2
ver|find "5.">nul&&goto :Admin
mshta vbscript:createobject("shell.application").shellexecute("%~s0","goto :Admin","","runas",1)(window.close)&goto :eof
:Admin
```
