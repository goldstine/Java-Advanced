# java8新特性
## Lambda表达式
+ 速度更快
+ 代码更少（增加了新的语法：Lambda表达式）
+ 强大的Stream api
+ 便于并行
+ 最大化减少空指针异常：Optional
+ Nashorn引擎，允许在jvm上运行js应用，在jdk的bin文件夹下有一个命令jjs jjs fun.js
```
package com.atguigu.gulimall.search.thread.newfeature;

import org.junit.Test;

import java.util.Comparator;

public class LambdaTest {
    @Test
    public void test1(){
        Runnable r1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("我爱北京天安门");
            }
        };
        r1.run();

        Runnable r2 = () ->System.out.println("我爱北京故宫");


        r2.run();

    }
    @Test
    public void test2(){
        Comparator<Integer> com1 = new Comparator<Integer>() {

            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(o1, o2);
            }
        };
        int compare = com1.compare(12, 21);
        System.out.println(compare);
        System.out.println("===============================");
        //lambda表达式的写法
        Comparator<Integer> com2 =  (o1,  o2)-> Integer.compare(o1, o2);

        int compare2 = com2.compare(32, 21);
        System.out.println(compare2);

        System.out.println("===============================");
        //方法引用的写法
        Comparator<Integer> com3 =  Integer::compare;

        int compare3 = com3.compare(32, 21);
        System.out.println(compare3);
    }
}
```
### lambda表达式的6种使用方式
```
package com.atguigu.gulimall.search.thread.newfeature;

import org.junit.Test;

import java.util.ArrayList;
import java.util.Comparator;
import java.util.function.Consumer;

/**
 * lambda表达式的使用
 * (1)举例：（o1,o2）->Integer.compare(o1,o2)
 * 2、格式：
 *      ->:lambda操作符或箭头操作符
 *      ->:左边：lambda形参列表（其实就是接口中的抽象方法的形参列表）
 *      ->右边：lambda体（其实就是重写的抽象方法的方法体）
 *（3）lambda表达式的使用（分为6种情况介绍）
 *
 * 如果左边只有一个参数，则可以省略小括号，如果可以进行类型推断，就可以省略类型
 * 如果有边只有一条语句，则可以省略大括号，省略大括号，return也必须省略
 *
 *（4）lambda表达式的本质：作为接口的实例
 */
public class LambdaTest1 {
    //语法格式一：无参无返回值的
    @Test
    public void test1(){
        Runnable r2 = () ->System.out.println("我爱北京故宫");
        r2.run();
    }

    //语法格式二：lambda需要一个参数，但是没有返回值
    @Test
    public void test2(){
        Consumer<String> con = new Consumer<String>() {

            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };
        con.accept("goldstine");
        System.out.println("===========================");

        //lambda表达式
        Consumer<String> con1=(String s)-> System.out.println(s);
        con1.accept("as");
    }
    //第三种语法格式：数据类型可以省略，因为可以由编译器推断出，称为类型推断
    @Test
    public void test3(){
        Consumer<String> con1=(s)-> System.out.println(s);
        con1.accept("as");
        //类型推断(1)
        int a[]=new int[]{1,2,3};
        //可以省略写成
        int b[]={1,2,3};

        //类型推断(2)
        ArrayList<String> arr=new ArrayList<>();
    }
    //第四种语法格式：lambda若只需一个参数时，参数的小括号可以省略
    @Test
    public void test4(){
        Consumer<String> con1=s-> System.out.println(s);
        con1.accept("as");
    }
    //语法格式五：lambda需要两个或两个以上的参数，多条执行语句，并且可以有返回值
    @Test
    public void test5(){
        Comparator<Integer> com1 = new Comparator<Integer>() {

            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(o1, o2);
            }
        };

        System.out.println("===============================");
        //lambda表达式的写法
        Comparator<Integer> com2 =  (o1,  o2)-> Integer.compare(o1, o2);

        int compare2 = com2.compare(32, 21);
        System.out.println(compare2);

        System.out.println("===============================");
        //方法引用的写法
        Comparator<Integer> com3 =  Integer::compare;

        int compare3 = com3.compare(32, 21);
        System.out.println(compare3);
    }
    //当lambda体只有一条语句时，return与大括号若有，都可以省略

}
```
## 函数式（Functional）接口
```
package com.atguigu.gulimall.search.thread.newfeature;

/**
 * 只包含一个抽象方法的接口称为函数式接口
 * 可以通过lambda表达式创建该接口的对象
 * lambda表达式的本质就是作为函数式接口的实例
 * 可以在接口上使用@FunctionalInterface注解，这样做就可以检查是否是一个函数式接口，同时javadoc也会包含一条声明，说明这个接口是一个函数式接口
 * 在java.util.function包下定义了java8的丰富的函数式接口
 *
 * java不但可以支持OOP，还支持OOF（面向函数编程）
 */
@FunctionalInterface
public interface MyInterface {
    void method();
}
```
### 四大核心函数式接口
```
package com.atguigu.gulimall.search.thread.newfeature;

import org.junit.Test;

import java.io.PrintStream;
import java.sql.SQLOutput;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Predicate;

/**
 * java内置的4大核心函数式接口
 * （1）消费型接口：Consumer<T>   void accept(T t)</T>
 * (2)供给型接口Supplier<T >       T get()</>
 * (3)Function<T,R>          R apply(T t)</T,R>
 * （4）断定型接口Predicate<T>     boolean test(T t)</T>
 *
 * 这4个函数式接口在实例化时都可以使用lambda表达式
 */
public class LmabdaTest2 {


    @Test
    public void test1(){
        happyTime(500, new Consumer<Double>() {
            @Override
            public void accept(Double aDouble) {
                System.out.println("学习太累了，去睡觉"+aDouble);
            }
        });
        //lambda
        happyTime(400,(aDouble)->System.out.println("学习太累了，去睡觉"+aDouble));
    }

    public void happyTime(double money, Consumer<Double> con){
        con.accept(money);
    }

    public ArrayList<String> filterString(List<String> list , Predicate<String> pre){
        //集合list根据某种规则进行过滤，具体是什么规则，则需要自己定义
        ArrayList<String> filterList=new ArrayList<>();
        for (String s : list) {
            if (pre.test(s)) {
                filterList.add(s);
            }
        }
        return filterList;
    }

    @Test
    public void test2(){
        List<String> list = Arrays.asList("北京", "南京", "天津", "东京", "西京", "普京");
        ArrayList<String> strings = filterString(list, new Predicate<String>() {
            @Override
            public boolean test(String s) {
                return s.contains("京");
            }
        });
        System.out.println(strings);
        //lambda
        ArrayList<String> list1 = filterString(list, s -> s.contains("京"));
        System.out.println(list1);
    }

    //方法引用,方法引用本质上就是lambda表达式
    //使用格式：类（对象）::方法名

    /**
     * 具体可以分为如下三种情况：
     * 对象::非静态方法
     * 类::静态方法
     * 类::非静态方法
     *
     */
    @Test
    public void test3(){
        Consumer<String> con1=str-> System.out.println(str);
        con1.accept("北京");
        System.out.println("================================");

        Consumer<String> con2=System.out::println;
        con2.accept("goldstine");

        PrintStream ps=System.out;
        Consumer<String> con3=ps::println;
        con3.accept("liulei");
    }

    //数组引用
    @Test
    public void test4(){
        //因为Function是函数式接口，所以可以使用lambda表达式作为其实例对象，即通过lambda表达式实现其唯一的一个抽象方法

        Function<Integer,String[]> func1= length->new String[length];
        String[] apply = func1.apply(5);
        System.out.println(Arrays.toString(apply));
        System.out.println("==============================");
        //数组引用
        Function<Integer,String[]> func2=String[]::new;
        String[] apply1 = func2.apply(6);
        System.out.println(Arrays.toString(apply1));
//可以把数组看成是一个特殊的类，则写法与构造器引用类似

    }

}
/**
 * 当要传递给lambda体的操作，已经由实现的方法了，可以使用方法引用
 * 方法引用就是lambda表达式，也就是函数式接口的一个实例，通过方法的名字来指向一个方法，可以认为是lambda表达式的一个语法糖
 *
 * lambda表达式，lambda表达式的6种写法，函数式接口
 * 其中Runnable接口额和Comparator接口都是函数时接口，
 * 所以只要是函数式接口就可以写成lambda表达式实现唯一的抽象方法，作为对应接口的实现类对象
 * 然后可以通过该对象调用该抽象放过发的具体实现
 * 更进一步的，可以将lambda写成方法引用，构造器引用，数组引用
 * 所以方法引用，构造器引用也是函数式接口的实例
 *
 */

```
## 方法引用与构造器引用


## stream api

## Optional类
