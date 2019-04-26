## 自定义异常


- 所有异常都必须是 Throwable 的子类
- 如果希望写一个检查性异常类，则需要继承 Exception 类
- 如果你想写一个运行时异常类，那么需要继承 RuntimeException 类

```java
class MyException extends Exception{}
```
例子
```java
import java.io.*;

//自定义异常类，继承 Exception 类
public class InsufficientFundsException extends Exception
{
    //此处的 amount 用来储存当出现异常 ( 取出钱多于余额时 ) 所缺乏的钱
    private double amount;

    public InsufficientFundsException(double amount) 
    {
        this.amount = amount;
    } 

    public double getAmount()
    {
        return amount;
    }
}
```
```java
import java.io.*;

//此类模拟银行账户
public class CheckingAccount
{
  //balance为余额，number为卡号
   private double balance;
   private int number;
   public CheckingAccount(int number)
   {
      this.number = number;
   }
  //方法：存钱
   public void deposit(double amount)
   {
      balance += amount;
   }
  //方法：取钱
   public void withdraw(double amount) throws
                              InsufficientFundsException
   {
      if(amount <= balance)
      {
         balance -= amount;
      }
      else
      {
         double needs = amount - balance;
         throw new InsufficientFundsException(needs);
      }
   }
  //方法：返回余额
   public double getBalance()
   {
      return balance;
   }
  //方法：返回卡号
   public int getNumber()
   {
      return number;
   }
}
```

```java
public class BankDemo
{
   public static void main(String [] args)
    {
        CheckingAccount c = new CheckingAccount(101);
        System.out.println("Depositing $500...");
        c.deposit(500.00);
        try {

            System.out.println("\nWithdrawing $100...");
            c.withdraw(100.00);
            System.out.println("\nWithdrawing $600...");
            c.withdraw(600.00);

        }catch(InsufficientFundsException e) {

            System.out.println("Sorry, but you are short $"
                                  + e.getAmount());
            e.printStackTrace();
        }
    }
}
```