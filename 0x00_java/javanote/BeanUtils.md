# BeanUtils
BeanUtils工具由Apache软件基金组织编写，提供给我们使用，主要解决的问题是：把对象的属性数据封装到对象中。在整个J2EE的编程过程中，我们经常会从各种配置文件中读取相应的数据，需要明白的一点是从配置文件中读取到的数据都是String，但是很显然我们的应用程序中不仅仅有String一种数据类型，比如：基本数据类型（int、double、char、float等），还有自定义数据类型（引用数据类型），那么我们必须面临的一个问题就是将字符串类型转换为各种具体的数据类型，该怎么办呢？有两种方法供我们是使用：

- 首先判断需要的数据类型，然后对字符串类型调用相应的方法，将其转换为我们想要的类型
- 使用BeanUtils工具

对于上面提到的两种方法，我们分析第一种存在的问题是太过于繁琐，每次都要进行大量的类型转换，Apache软件基金会给我们提供了第二种方法，使用其提供的BeanUtils工具，具体的说只需要知道其中的两个方法就能实现类型的转换，很简单，降低了编程的难度。

## 常用方法
Beanutils工具在使用时几乎只用到以下几个方法，其中一个方法通常情况下都是使用匿名内部类。
```java
BeanUtils.setProperty(bean, name, value);
// 其中bean是指你将要设置的对象，name指的是将要设置的属性（写成”属性名”）,
// value（从配置文件中读取到到的字符串值）

BeanUtils.copyProperties(bean, name, value)
// 和上面的方法是完全一样的。使用哪个都可以

ConvertUtils.register(Converter converter , ..)
// 当需要将String数据转换成引用数据类型（自定义数据类型时），需要使用此方法实现转换。

BeanUtils.populate(bean,Map)
// 其中Map中的key必须与目标对象中的属性名相同，否则不能实现拷贝。

BeanUtils.copyProperties(newObject,oldObject)
// 实现对象的拷贝
```
自定义数据类型使用BeanUtils工具时，本身必须具备getter和setter方法，因为BeanUtils工具本身也是一种内省的实现方法，所以也是借助于底层的getter和setter方法进行转换的。

## 例子
```java
// 想要封装成javabean的对象
package com.jpzhutech.beanutils;

import java.util.Date;

import javax.xml.crypto.Data;

public class Emp {
    private int id ;
    private String name;
    public Emp(int id, String name, double salary, Date date) {
        super();
        this.id = id;
        this.name = name;
        this.salary = salary;
        this.date = date;
    }

    private double salary;
    private Date date;


    public Date getDate() {
        return date;
    }
    public void setDate(Date date) {
        this.date = date;
    }
    public Emp() {

    }

    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public double getSalary() {
        return salary;
    }
    public void setSalary(double salary) {
        this.salary = salary;
    }


    @Override
    public String toString() {
        // TODO Auto-generated method stub
        return "编号："+this.id+" 姓名："+this.name+" 工资:"+this.salary+" 生日："+date;
    }

}
```
```java
// 使用BeanUtils组件进行转换
/**
 * BeanUtils工具的使用
 * 功能：BeanUtils主要是用于将对象的属性封装到对象中
 * BeanUtils的好处：
 * BeanUtils设置属性值的时候，如果属性是基本数据类型，那么BeanUtils会自动帮我们进行数据类型的转换，并且
 * BeanUtils设置属性的时候也是依赖于底层的getter和setter方法
 * 
 * 如果设置的属性值是其他的引用数据类型，此时必须要注册一个类型转换器才能实现自动的转换
 * */

package com.jpzhutech.beanutils;

import java.lang.reflect.InvocationTargetException;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

import javax.xml.crypto.Data;

import org.apache.commons.beanutils.BeanUtils;
import org.apache.commons.beanutils.ConvertUtils;
import org.apache.commons.beanutils.Converter;
import org.apache.commons.beanutils.locale.converters.DateLocaleConverter;

public class TestBeanUtils {
    public static void main(String[] args) throws IllegalAccessException, InvocationTargetException {
        //从文件中读取到的数据都是字符串的数据，或者是表单提交的数据获取到的时候也是字符串数据
        //在J2EE的编程中，我们会通过配置文件或者直接从文件获取数据的方式得到我们想要的数据
        //那么就存在一个问题，当我们需要的是一个int时，读到的数据确是String，那么我们每次是不是都要先判断实际
        //需要的是什么数据类型，然后进行一个强制的类型转换呢？回答是不需要，我们借助Apache软件基金会提供的BeanUtils工具
        //根本不用管什么样的数据类型，只需要使用BeanUtils的setProperties方法，该方法有三个参数，对三个参数进行设置便会
        //实现自动的数据类型转换

        /*ConvertUtils.register(new Converter() {


            //自定义日期类型转换器
            @Override
            public Object convert(Class  type, Object value) { //type:目前需要转换的数据类型 value：目前参数的值
                //目标：将字符串转换为日期

                if(type != Date.class)  return null;

                if (value == null || "".equals(value.toString().trim())) {
                    return null;
                }
                SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy年MM月dd日");
                Date date = null;
                try {
                     date = dateFormat.parse((String)value);
                } catch (ParseException e) {
                    throw new RuntimeException(e);
                }
                return date;
            }
        }, Date.class);  //Date.class表示要转换的成引用类型，Date类型不是基本数据类型，所以需要一个转换器进行相应的转换，同样该功能属于BeanUtils
        */

        //使用日起转换器工具类
        ConvertUtils.register(new DateLocaleConverter(), Date.class);   //不灵活，自己实现最好

        String id ="110";  //我们用这个三个String类型的属性代表从配置文件中读取到的数据，实际编程过程中这些数据直接从properties文件中读取
        String name = "朱君鹏";
        String salary = "1000";
//        String birthday = "2015年01月30日"; //如果要使用工具中提供的转换器必须要符合一定的格式，像这种格式就不能实现转换
        String birthday = "2015-01-30";  //该格式可以实现使用工具提供的转换器类将字符串正确的转换，


        Emp p = new Emp();  //读取到数据之后，对该对象的属性进行设置，使用BeanUtils工具可以避免强制类型的转换，但是在Emp类中的每个属性都要有getter和setter方法
                             //因为BeanUtils工具实际上是对内省的封装，使其更加的好用，所以其底层还是依赖getter和setter方法

        BeanUtils.setProperty(p, "id", id);  //其中p代表的是要设置的对象

        BeanUtils.setProperty(p, "name", name);   //中间一个参数代表的是要设置的属性

        BeanUtils.setProperty(p, "salary", salary); //第三个参数代表的是第二个属性的值

        BeanUtils.setProperty(p, "date", birthday);

        System.out.println(p);
    }
}
```




[参考链接](https://blog.csdn.net/jpzhu16/article/details/51582930)