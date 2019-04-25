## 条件判断
```java
if( boolean_expression )
{
   /* 如果布尔表达式 boolean_expression 为真将执行的语句 */
}

if(布尔表达式){
   //如果布尔表达式的值为true
}else{
   //如果布尔表达式的值为false
}

if(布尔表达式 1){
   //如果布尔表达式 1的值为true执行代码
}else if(布尔表达式 2){
   //如果布尔表达式 2的值为true执行代码
}else if(布尔表达式 3){
   //如果布尔表达式 3的值为true执行代码
}else {
   //如果以上布尔表达式都不为true执行代码
}
```
```java
switch(expression){
    case value :
       //语句
       break; //可选
    case value :
       //语句
       break; //可选
    //你可以有任意数量的case语句
    default : //可选
       //语句
}
```
- switch 语句中的变量类型可以是： byte、short、int 或者 char从 Java SE 7 开始，switch 支持字符串类型了
- 当变量的值与 case 语句的值相等时，那么 case 语句之后的语句开始执行，直到 break 语句出现才会跳出 switch 语句
- 当遇到 break 语句时，switch 语句终止,如果没有 break 语句出现，程序会继续执行下一条 case 语句，直到出现 break 语句
- switch 语句可以包含一个 default 分支，该分支必须是 switch 语句的最后一个分支
default 在没有 case 语句的值和变量值相等的时候执行
default 分支不需要 break 语句

## 循环语句
```java
while( 布尔表达式 ) {
  // 循环体
}

do {
    //代码语句
}while(布尔表达式);

for(初始化; 布尔表达式; 更新) {
    // 代码语句
}
for(int x = 10; x < 20; x = x+1) {
         System.out.print("value of x : " + x );
         System.out.print("\n");
      }

// 主要用于数组的增强型 for 循环
for(声明语句 : 表达式)
{
   // 代码句子
}
int [] numbers = {10, 20, 30, 40, 50};
for(int x : numbers ){
    System.out.print( x );
    System.out.print(",");
}
```