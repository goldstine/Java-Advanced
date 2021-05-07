# 如何自定义枚举类
（1）通过class的方式定义枚举类：属性private final ;属性作为final常量的赋值是通过private的构造函数实现的；然后提供当前枚举类的多个对象public static final 枚举类名 对象名；提供get/set/toString

# 如何使用关键字enum定义枚举类
(2)通过enum关键字创建枚举类:首先要定义对象，通过逗号隔开，最后一个对象通过分号隔开

# Enum类的主要方法
toString()输出枚举类的名称；values()输出枚举类的所有对象；valuesof("WINTER")将字符串转换为枚举类对象，或者叫通过字符串获得对应的枚举类对象，如果该名称没有对应的枚举类对象，则抛出异常

# 实现接口的枚举类
(1)直接实现接口对应的抽象方法，则在调用该接口的抽象方法时，所有的对象调用的方法都是一样的
（2）如果需要是每一个对象调用该接口的抽象方法多不一样，则直接在枚举类中的对象后面写上{抽象方法的实现}
## 枚举类
```
package com.atguigu.gulimall.search.thread.EnumTest;

/**
 * 当一个类的对象是有限的，则称该类为枚举类
 *当需要定义自组常量时，强烈建议使用枚举类
 * 如果枚举类自由一个对象，则可以作为单例模式的实现方式
 * （一）如何定义枚举类
 * 方式一：jdk5.0之前，自定义枚举类
 * 方式二：jdk5.0，可以使用enum关键字定义枚举类,定义的类默认继承自java.lang.Enum类
 *(二)Enum类中的常用方法
 * values()方法：返回枚举类的对象数组，该方法可以很方便地遍历所有的枚举值
 * valuesOf(String str):可以把字符串转为对应的枚举类对象，要求字符串必须是枚举类对应的名称
 * toString():返回当前枚举类对象常量的名称
 * (三)使用enum关键字定义的枚举类实现接口的情况
 * 情况一：实现接口，再enum类中实现抽象方法
 * 情况二：实现接口方法的时候，实现对不同对象调用的接口方法是不一样的
 *
 */
public class EnumClassTest {
    public static void main(String[] args) {
        Season spring = Season.SPRING;
        System.out.println(spring);
        System.out.println(spring.toString());
        System.out.println(Season.class.getSuperclass());//class java.lang.Enum
        //values():
        Season[] values= Season.values();
        for (int i = 0; i < values.length; i++) {
            System.out.println(values[i]);
            values[i].show();
        }
        //线程状态的枚举
//        Thread.State[] values1 = Thread.State.values();
//        for (int i = 0; i < values1.length; i++) {
//            System.out.println(values1[i]);
//        }

        //valueOf(String objName)：根据对象的名称找到对应的对象  返回枚举类中对象名是objName的对象
        //如果没有objName的枚举类对象，则抛异常IllegalArgumentException
        Season winter = Season.valueOf("WINTER");
        System.out.println(winter);
        winter.show();
    }
}

//接口
interface info{
    void show();
}

//定义枚举类
class Season1{
    //声明Season对象的属性：private final修饰
    private final String seasonName;
    private final String seasonDesc;

    //首先需要私有化类的构造器,并给对象属性赋值
    private Season1(String seasonName,String seasonDesc){
        this.seasonName=seasonName;
        this.seasonDesc=seasonDesc;
    }

    //提供当前枚举类的多个对象
    public static final Season1 SPRING=new Season1("春天","春暖花开");
    public static final Season1 SUMMER=new Season1("夏天","夏日炎炎");
    public static final Season1 AUTUMN=new Season1("秋天","秋高气爽");
    public static final Season1 WINTER=new Season1("冬天","冰天雪地");
    //获取枚举类对象的属性

    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }

    @Override
    public String toString() {
        return "Season{" +
                "seasonName='" + seasonName + '\'' +
                ", seasonDesc='" + seasonDesc + '\'' +
                '}';
    }
}
enum Season implements info{
    //首先需要私有化类的构造器,并给对象属性赋值
    //提供当前枚举类的多个对象
    SPRING("春天","春暖花开"){
        @Override
        public void show() {
            System.out.println("春天在哪里");
        }
    },
    SUMMER("夏天","夏日炎炎"){
        @Override
        public void show() {
            System.out.println("夏天在哪里");
        }
    },
    AUTUMN("秋天","秋高气爽") {
        @Override
        public void show() {
            System.out.println("秋天在哪里");
        }
    },
    WINTER("冬天","冰天雪地") {
        @Override
        public void show() {
            System.out.println("大约在冬季");
        }
    };

    private final String seasonName;
    private final String seasonDesc;


    private Season(String seasonName,String seasonDesc){
        this.seasonName=seasonName;
        this.seasonDesc=seasonDesc;
    }
    //获取枚举类对象的属性

    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }

    @Override
    public String toString() {
        return "Season{" +
                "seasonName='" + seasonName + '\'' +
                ", seasonDesc='" + seasonDesc + '\'' +
                '}';
    }

//    @Override
//    public void show() {
//        System.out.println("这是一个季节");
//    }
}
```
# 注解
（1）文档注解
（2）主要使用的注解：@Override @SuppressedWarn....
(3)自定义注解   自定义注解的成员 String[] values defalut "";  @interface
（4）元注解 @Retention  @Target @Document @Inherited注解
（5）jdk8的注解 ：可重复注解：通过数组的方式 在注解@MyAnnotation中添加上@Repeatable(MyAnnotation.class)
类型注解 TYPE_PARAMETER
TYPE_USE  可以在泛型中使用注解
```
package com.atguigu.gulimall.search.thread.EnumTest;

import org.junit.Test;

import java.lang.annotation.Annotation;
import java.util.ArrayList;

/**
 * 注解：jdk5.0新增的功能
 * (一)生成文档相关的注解
 * （二）在编译时进行格式检查（JDK内置的三个基本注解）@Override  @Deprecated  @SuppressWarnings:抑制编译器警告
 * (三)如何自定义注解：参照SuppressWarnings定义注解
 *注解声明为@interface
 * 自定义注解自动继承了java.lang.annotation.Annotation接口
 * Annotation的成员变量再Annotation定义中以无参数的方法的形式来声明，其方法名和返回值定义了该成员的名字和类型，我们称为配置参数
 * 类型只能是八种基本数据类型，String,Class,enum,Annotation类型
 *对于没有成员变量的注解，称为一个标记，比如@Override就是一个标记,就比如序列化的接口Seriable内部也没有定义抽象方法，所以称Seriable接口为标识接口
 *
 * 如果注解有成员，在使用注解时，需要指定成员的值
 * 自定义注解必须配上注解的信息处理流程（使用反射）才有意义
 * （四）jdk提供的4种元注解 ，元注解就是修饰其他注解的注解  就比如数据库中的元数据，修饰其他数据的数据
 * 对现有注解进行修饰的注解
 * @Retention  @Target  @Documented @Inherited
 * @Retention:表明注解的生命周期  SOURCE,CLASS(默认行为，默认的注解的生命周期),RUNTIME（只有被声明为RUNTIME生命周期的注解才能够通过反射获得）
 *
 * @Target 用于被修饰的注解可以用于修饰哪些元素
 *
 * 自定义注解通常会指定两个元注解：@Retention指定生命周期 @Target指定作用范围
 *
 * @Document  ：默认情况下javaDoc是不会保留注解的，Document所修饰的注解在被javadoc解析时，保留下来
 * @Deprecated注解就使用了@Document注解，所以在通过javadoc生成接口文档的时候就会有@Deprecated注解
 *
 * @Inherited：被它修饰的注解将具有继承性  如果在自定义的注解上加上@Inherited，那么在使用的时候如果在Person父类上加上该注解，那么再它的子类上也就自动具有了该注解
 *
 * (五)通过反射获取注解信息
 *clazz.getAnnotations(),返回一个数组，有多个注解
 *
 * （六）jdk8中注解的新特性   可重复注解，类型注解
 *可重复注解：在MyAnnotation上声明一个@Repeatable(MyAnnotations.class)  MyAnnotation的Target和Retention等元注解（@Inherited）需要和MyAnnotations相同
 *
 *类型注解：
 *ElementType.TYPE_PARAMENTER 表示该注解能写在类型变量的声明语句中（如：泛型声明）
 * ElementType.TYPE_USE  表示该注解能写在使用类型的任何语句中
 *
 */
public class AnnotationTest {

    @Test
    public void test1(){
        Class clazz= Student.class;
        Annotation[] annotations = clazz.getAnnotations();
        for (int i = 0; i < annotations.length; i++) {
            System.out.println(annotations[i]); //输出：@com.atguigu.gulimall.search.thread.EnumTest.MyAnnotation(value=ho)
            //这是由于Student继承了Person的注解MyAnnotation注解
        }

    }
}
//@MyAnnotation(value="ho")  //如果注解只有一个成员，则可以省略value=
//@MyAnnotations({@MyAnnotation(value="ho"),@MyAnnotation(value="ho")})//这个是jdk8之前定义重复注解的方式，通过注解数组实现
@MyAnnotation(value="12")
@MyAnnotation(value="abc")   //实现可重复注解
class Person{
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public void walk(){
        System.out.println("人走路");
    }
    @SuppressWarnings("unused")
    public void eat(){
        System.out.println("人吃饭");
    }


}
class Student extends Person{

    @Override
    public void walk() {
        System.out.println("学生走路");
    }
}

class Generic<@MyAnnotation T>{    //表示修饰泛型的注解 @MyAnnotation
    public void show() throws @MyAnnotation RuntimeException{
        ArrayList<@MyAnnotation String> list=new ArrayList<>();
        int num=(@MyAnnotation int)10L;

    }
}
```

