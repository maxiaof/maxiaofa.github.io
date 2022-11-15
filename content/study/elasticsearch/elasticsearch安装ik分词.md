+++
title = "Elasticsearch安装ik分词"
chaper = true
+++

# Elasticsearch安装ik分词
## 1.下载分词插件
下载地址： [elasticsearch-analysis-ik][https://github.com/medcl/elasticsearch-analysis-ik/releases] 下载版本需要与安装的es版本相同

## 2.安装ik
```解压-->将文件复制到 es的安装目录/plugin/ 重启既可生效不需要修改配置文件```

![elasticsearch安装ik分词-安装位置](http://blog.maxiaofa.com/static/img/elasticsearch安装ik分词-安装位置.png)


Tips：
``` shell
#下载命令 
curl -o 下载文件名.zip -L 直链 
curl -o ik.zip -L https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.17.1/elasticsearch-analysis-ik-7.17.1.zip

#解压命令： 
unzip 文件名.zip 文件夹 
unzip ik.zip ik
```
