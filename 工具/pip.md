# pip

---
## 基本操作
### 安装包
```shell
$ pip install SomePackage            # latest version
$ pip install SomePackage==1.0.4     # specific version
$ pip install 'SomePackage>=1.0.4'     # minimum version
```
### 从需求文件安装
```shell
pip install -r requirements.txt
```
### 生成需求文件
```shell
pip freeze > requirements.txt
```
### 约束文件
```shell
pip install -c constraints.txt
```
### 从Wheel安装
```shell
pip install SomePackage-1.0-py2.py3-none-any.whl
```
>whl格式本质上是一个压缩包，里面包含了py文件，以及经过编译的pyd文件。使得可以在不具备编译环境的情况下，选择合适自己的python环境进行安装。
Wheel是已经构建过的包程序，使用Wheel包来安装第三方包可以极大的加快安装的速度，并且降低安装的难度。
### 为你的requirements及其所有依赖项构建Wheels到本地目录
```shell
pip install wheel
pip wheel --wheel-dir=/local/wheels -r requirements.txt
```
### 用Wheels的本地目录（而不是从PyPI中）安装了这些requirements
```shell
pip install --no-index --find-links=/local/wheels -r requirements.txt
```
### 卸载包
```shell
pip uninstall SomePackage
pip uninstall -r requirements.txt
```
### 列出包
```shell
pip list
列出过时的软件包，并显示可用的最新版本
pip list --outdated
显示已安装包的详细信息
pip show sphinx
```
### 搜索包
```shell
pip search "query"
```
### 指定源
```shell
pip3 install -i https://pypi.doubanio.com/simple/ selenium
```
---
## 配置选项
---
## 从本地包安装
### 保存包到本地
```shell
pip install --download DIR -r requirements.txt
```
>上述命令在从pypi下载前会检查本地是否有wheel缓存，如果你之前没有在本地安装过需求文件中的某些包，将不会生成他们的wheel缓存，若你需要wheels
```shell
pip wheel --wheel-dir DIR -r requirements.txt
```
### 仅从本地安装
```shell
pip install --no-index --find-links=DIR -r requirements.txt
```
---
## 递归升级(仅在需要时)
```shell
pip install --upgrade packge --upgrade-strategy
```
>--upgrade-strategy 有两个选项
eager：升级所有依赖项，无论它们是否仍满足新的父级要求
only-if-needed：仅在不满足新父要求时才升级依赖项(默认)
---
## 用户安装
```shell
要将“SomePackage”安装到将site.USER_BASE自定义为“/ myappenv”的环境中
export PYTHONUSERBASE=/myappenv
pip install --user SomePackage
```
```
pip install --user 遵循四条规则：
1. 当全局安装的软件包位于python路径上，并且它们 与安装要求冲突时，它们将被忽略，而不会被 卸载。
2. 当全局安装的软件包位于python路径上并且它们满足 安装要求时，pip什么都不做，并报告满足要求（类似于在--system-site-packages virtualenv中安装软件包时全局软件包如何满足要求）。
3. 由于用户站点不在python路径上，因此pip不会--user在--no-site-packagesvirtualenv（即默认类型的virtualenv）中执行安装。安装没有意义。
4. 在--system-site-packagesvirtualenv中，pip不会安装与virtualenv site-packages中的包冲突的包。-user安装缺少sys.path优先级并且毫无意义。
```
---
[参考](https://pip.pypa.io/en/stable/user_guide/)


