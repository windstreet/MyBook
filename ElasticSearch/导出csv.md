# 导出csv

## 一、使用 `es2csv` 从 `ES` 导出csv文件

- **安装**   
选择一个常用的虚拟环境安装 `es2csv` 库
```bash
pip install es2csv
```

- **导出csv文件**  
在安装有 `es2csv` 库的虚拟环境中执行如下命令：     

```bash
es2csv -u localhost:9600 -i crawler-result_amazon_product-2018.07.03 -q '*' -o /Users/admin/Downloads/database3.csv

es2csv -u http://192.168.1.10:19200 -i crawler-amazon_bad_review_ca -q '*' -o /Users/linrenwei/Desktop/ca.csv
```


> 注解：  
    从 `localhost:9600`          
    导出索引为 `crawler-result_amazon_product-2018.07.03` 的所有数据    
    并且保存为csv文件到指定路径 `/Users/admin/Downloads/`  下
    
---


[源码](https://github.com/taraslayshchuk/es2csv)
