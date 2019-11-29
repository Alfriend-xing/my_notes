# shell语法

## 常用语法

```sh
# 打印字符
echo "hello."

# 打印时间，可以设置显示格式和增减时间，此处略
date

#变量
d=“hello.”
# echo $d打印hello.
# 直接调用$d执行hello.

#命令变量
os_version=`ls`
os_version=$(ls)
# echo $os_version 打印ls执行结果
# 调用$os_version 执行ls执行结果

# 启动参数
echo "$1"
echo "$2"
# 调用 sh test.sh val1 val2
# $0为脚本文件名称

# 读取输入
echo "inopt a:"
read a
# 或者
read -p "input b:" b
```

## 判断语句

### if语句

```sh
read a
if ((a<10)); then

command

fi
```

```sh
if ((a<10)) ; then

command

else

command

fi
```

```sh
if ((a<10)) && ((a>0)); then

command

elif ((a<20)); then

command

else

command

fi
```

更复杂的条件使用`[]`

```sh
read a
if [$a -lt 5]; then

command

fi
```
- 数值判断 `if [$a -gt 1]`
    - `-lt` （小于）
    - `-gt` （大于）
    - `-le`（小于等于）
    - `-ge` （大于等于）
    - `-eq` （等于）
    - `-ne` （不等于）
- 字符串判断 `if test $num1 = $num2`
  - `=` 字符串等于则为真
  - `!=` 字符串不相等则为真
  - `-z` 字符串的长度为零则为真
  - `-n` 字符串的长度不为零则为真
- `&&` `||` 与或
- 文件判断 `if [ -e filename ]`
  - `-e` ：判断文件或目录是否存在
  - `-s` ：判断文件不为空
  - `-d` ：判断是不是目录，并是否存在
  - `-f` ：判断是否是普通文件，并存在
  - `-r` ：判断文档是否有读权限
  - `-w` ：判断是否有写权限
  - `-x` ：判断是否可执行


```sh
# 例 判断/home是否是文件夹
read a
if [-d /home]; then

command

fi
```

### case语句

```sh
read a

case $a in
value1)
    command
    command
    ;;
value2)
    command
    ;;
value3)
    command
    command
    ;;
# default
*)
    command
    ;;
esac
```

## 循环语句

### for循环

```sh
for 变量名 in 循环的条件； do

command

done
```

```sh
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

```sh
# 单行表示
for var in item1 item2 ... itemN; do command1; command2… done;
```

```sh
#例

for i in `seq 1 5`; do
    echo $i
done

# 例
for str in 'This is a string'
do
    echo $str
done

#例
for((i=1;i<=5;i++));do
    echo "这是第 $i 次调用";
done;
```

### while 循环

```sh
while 条件; do

command

done
```

```sh
# 例
a=10

while [$a -ge 1]; do
    echo "$a"
    a=$[$a-1]
done
```

```sh
# 监控脚本常用的死循环
while :; do

command

done
```

## 函数

必须先定义后调用

```sh
[ function ] funname ()

{

    action;

    [return int;]

}
```
- 可以带function fun() 定义，也可以直接fun() 定义,不带任何参数。
- 参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。


```sh
# 定义
demoFun(){
    echo "这是我的第一个 shell 函数!"
}
# 调用
demoFun
```

```sh
# 获取返回值
funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```
- 函数返回值在调用该函数后通过 $? 来获得

```sh
# 函数参数
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}

# 调用时传入参数
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```
- $10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数
- 不知函数中如何区分脚本传入参数和函数调用参数？













参考地址：
- https://www.cnblogs.com/zhang-jun-jie/p/9266858.html
- https://www.runoob.com/linux/linux-shell-func.html