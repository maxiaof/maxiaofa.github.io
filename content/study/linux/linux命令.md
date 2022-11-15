+++
title = "Linux命令"
chaper = true
+++

# Linux命令

## 1.查询内存占比

``` bash
free -m | sed -n '2p' | awk '{print "当前使用 "$3"M,最大内存 "$2"M,百分比 "$3/$2*100"%"}'
```

