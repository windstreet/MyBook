# Linux

将目录内的所有文件合并为一个文件
```bash
for f in `ls`; do tail -n +2 "$f" >> combined.csv; done;
```

type 命令用于查找 Linux 命令的信息  
```bash
type -a ls  # 等效于 `type ls && type -p ls`
```
