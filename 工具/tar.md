# tar

### 创建归档文件(tar)
```shell
tar -cvf tecmint-14-09-12.tar /home/tecmint/
```
- c 创建归档文件
- v 显示过程
- f 设置归档文件名

### 创建归档压缩文件(tar.gz)
```shell
tar cvzf MyImages-14-09-12.tar.gz /home/MyImages
或者
tar cvzf MyImages-14-09-12.tgz /home/MyImages
```
### 创建归档压缩文件(tar.bz2)
```shell
tar cvfj Phpfiles-org.tar.bz2 /home/php
OR
tar cvfj Phpfiles-org.tar.tbz /home/php
OR 
tar cvfj Phpfiles-org.tar.tb2 /home/php
```
### 解压归档文件(tar)
```shell
tar -xvf public_html-14-09-12.tar -C /home/public_html/videos/
```
> 可以不指定目录

### 解压文件(tar.gz)
```shell
tar -zxvf thumbnails-14-09-12.tar.gz
```
- z 解gz压缩
- x 解tar归档
- v 显式执行
- f 指定文件名

### 解压文件(tar.bz2)
```shell
tar -jxvf videos-14-09-12.tar.bz2
```

### 列出文件内容(不解压)(tar)
```shell
tar -tvf uploadprogress.tar
```

### 列出文件内容(不解压)(tar.gz)
```shell
tar -tvf staging.tecmint.com.tar.gz
tar -tvf Phpfiles-org.tar.bz2
```
### 解压其中一个文件
```shell
tar -xvf cleanfiles.sh.tar cleanfiles.sh
OR
tar --extract --file=cleanfiles.sh.tar cleanfiles.sh

tar -zxvf tecmintbackup.tar.gz tecmintbackup.xml
OR
tar --extract --file=tecmintbackup.tar.gz tecmintbackup.xml

tar -jxvf Phpfiles-org.tar.bz2 home/php/index.php
OR
tar --extract --file=Phpfiles-org.tar.bz2 /home/php/index.php
```
### 解压部分文件
```shell
tar -xvf tecmint-14-09-12.tar "file 1" "file 2" 

tar -zxvf MyImages-14-09-12.tar.gz "file 1" "file 2" 

tar -jxvf Phpfiles-org.tar.bz2 "file 1" "file 2"
```
### 使用通配符提取一类文件
```shell
tar -xvf Phpfiles-org.tar --wildcards '*.php'

tar -zxvf Phpfiles-org.tar.gz --wildcards '*.php'

tar -jxvf Phpfiles-org.tar.bz2 --wildcards '*.php'
```

### 添加文件或文件夹到归档压缩文件根目录
```shell
tar -rvf tecmint-14-09-12.tar xyz.txt

tar -rvf tecmint-14-09-12.tar php

tar -rvf MyImages-14-09-12.tar.gz xyz.txt

tar -rvf Phpfiles-org.tar.bz2 xyz.txt
```
- r 添加

### 验证文件完整性
>无法验证压缩文件(gz bz2)
```shell
tar tvfW tecmint-14-09-12.tar
```
### 查看文件大小
>单位KB
```shell
tar -czf - tecmint-14-09-12.tar | wc -c
12820480

tar -czf - MyImages-14-09-12.tar.gz | wc -c
112640

tar -czf - Phpfiles-org.tar.bz2 | wc -c
20480
```

[参考链接](https://www.tecmint.com/18-tar-command-examples-in-linux/)