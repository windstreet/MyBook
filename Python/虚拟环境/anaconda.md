# anaconda

注：Miniconda 是 anaconda 较小的发行版（仅包含 conda 和 Python），该文使用的是 miniconda。


## 1）Anaconda是什么？

    Anaconda 附带了一大批常用数据科学包，它附带了 conda、Python 和 150 多个科学包及其依赖项。
    Anaconda 的 包管理功能 是在 conda（一个包管理器和环境管理器）上发展出来的。
    conda 是开源包（packages）和虚拟环境（environment）的管理系统。

---

## 2）如何安装Anaconda？

### 2.1.1）方式一：运行桌面上的 Miniconda3-latest-MacOSX-x86_64.sh 脚本：
    
    cd ~/Desktop  
    bash Miniconda3-latest-MacOSX-x86_64.sh 
    
### 2.1.2）方式二：下载 .pkg 文件

    下载之后安装就可以了（可以全部选默认设置）。

### 2.2）激活conda

    要先将 "/Users/admin/miniconda3/bin" 这个地址添加到环境变量 PATH 里才能运行 conda 这个命令。
    
    export PATH="/Users/admin/miniconda3/bin:$PATH"    # $

### 2.2.1）每次都运行 export PATH="..." 上述这个命令会很繁琐

    现将它【miniconda3程序的路径】添加到 zsh 配置文件 中：
    vim ~/.zshrc 
    进入文件后，将 export PATH="/Users/admin/miniconda3/bin:$PATH" 写入文件的最后一行，保存退出（ :x! ）。 # $

   #### 补充说明：
   >1）`～`  表示你的home目录，在OSX下位于 `/Users/你的用户名/`  
   >2）`.`   类unix下的隐藏文件，文件名带 `.` 之后在GUI文件管理器和 `ls` 的默认设置下不 会显示出来，使用 `ls -a` 命令可以显示出这些文件。  
   >3）`zshrc`  是一个文件，准确的说这个文件的文件名是 `.zshrc`  
   >4）`~/.zshrc`  这就是个配置文件的路径而已。  

可参考：https://segmentfault.com/q/1010000004376181/a-1020000004377872 


参考：  
[miniconda的安装及使用](https://blog.csdn.net/weixin_42066885/article/details/80323173)  
[mac安装Anaconda无法使用conda命令怎么办？](https://zhuanlan.zhihu.com/p/61717000)

---

## 3）管理包
	
   ```conda``` 的包管理功能和 ```pip``` 是一样的，选择 pip 来安装包也是没问题的。（conda 有时安装不了的包，可以用 pip 来安装，反之亦然）

### 3.1）安装包

    conda install package_name		   
	conda install pandas numpy		# 同时安装多个包
	conda install numpy=1.10	    # 指定包版本
			   						# conda 还会自动为你安装该包的所有依赖项

### 3.2）卸载包

    conda remove package_name

### 3.3）更新包
		
    conda update package_name
    conda update --all	 	# 更新所有包

### 3.4）列出已安装的包
	
    conda list

### 3.5）列出该包所有版本
	
    conda search package_name

---

## 4）管理虚拟环境

### 4.0）当前虚拟环境的信息

    conda info

### 4.1）创建环境
	
    conda create -n env_name python=3.7.3	# /Users/admin/miniconda3/envs/env_name

### 4.2）进入 / 离开环境
	
    source activate env_name    # conda activate env_name
	source deactivate           # conda deactivate

### 4.3）共享环境
	
    conda env export > environment.yaml		# 将你当前的环境保存到文件中包保存为YAML文件（包括Pyhton版本和所有包的名称）。
	conda env update -f=/path/to/environment.yaml		# 更新你的环境（首先要进入待更新的环境）
		
    类似于：
	pip freeze > environment.txt
	pip install -r /path/to/requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple --no-cache-dir	                 # -i 可选项，用于指定包的来源

### 4.4）列出所有conda创建的虚拟环境
	
    conda info -e
	conda env list

	其中base为默认的环境（即当你不在选定环境中时所使用的环境，一安装 conda 就有该环境，类似于系统自带的python一样。）。

### 4.5）删除环境

    conda remove -n env_name
	conda env remove -n env_name

---

### 安装nb_conda用于notebook自动关联nb_conda的环境。


    conda install pytorch -c pytorch		# 安装 pytorch
    pip install torchvision				# torchvision 是torch提供的计算机视觉工具包

## 注意
```bash
conda config --prepend envs_dirs /Users/(my name)/anaconda3/envs
```

[Could not find conda environment](https://stackoverflow.com/questions/58369030/could-not-find-conda-environment)
