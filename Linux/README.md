# Linux

将目录内的所有文件合并为一个文件
```bash
for f in `ls`; do tail -n +2 "$f" >> combined.csv; done;
```

type 命令用于查找 Linux 命令的信息  
```bash
type -a ls  # 等效于 `type ls && type -p ls`
```


## 学习资料

[10 分钟学会Linux常用 bash命令](https://www.cnblogs.com/savorboard/p/bash-guide.html#a)  
[10 分钟学会Linux常用 bash命令 - git源](https://github.com/vuuihc/bash-guide#11-file-operations)
