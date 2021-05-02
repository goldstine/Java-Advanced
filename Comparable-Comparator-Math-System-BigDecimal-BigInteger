```
package com.atguigu.gulimall.search.thread.Date;

import org.junit.Test;

import java.math.BigDecimal;
import java.math.BigInteger;
import java.util.Arrays;
import java.util.Comparator;

/**
 *说明：java中的对象，正常情况下，智能进行比较：==或！=。不能和使用>或<的
 * 但是在开发场景中，需要对多个对象进行排序，言外之意，就需要比较对象的大小
 * 如何实现？使用两个接口中的任何一个：Comparable或Comparator
 */
public class CompareTest {

    /**
     * Comparable接口的使用
     * 像String和包装类都实现了Comparable接口，进而实现类CompareTo(),给出了比较两个对象的方式
     * 重写CompareTo()的规则：
     * （1）如果当前对象this大于形参对象obj，则返回正整数
     * （2）如果当前对象this小于形参对象obj，则返回负整数，如果当前对象this等于形参对象，则返回0
     * 默认情况下是从小到大的方式进行排序
     *对于自定义类，如果需要排序，则可以让自定义类实现Comparable接口，重写compareTo()
     */
    @Test
    public void test1(){
        String[] arr=new String[]{"AA","CC","MM","GG","JJ","DD","KK"};
        Arrays.sort(arr);//String实现了Comparable接口
        System.out.println(Arrays.toString(arr));
    }

    @Test
    public void test2(){
        Goods[] arr=new Goods[4];
        arr[0]=new Goods("lenovoMouse",34);
        arr[1]=new Goods("dellMouse",43);
        arr[2]=new Goods("xiaomiMouse",12);
        arr[3]=new Goods("huaweiMouse",65);

        Arrays.sort(arr);
        System.out.println(Arrays.toString(arr));
    }

    /**
     * Comparator接口的使用：定制排序
     * 说明：
     * 当元素的类型没有实现java.lang.Comparable接口而又不方便修改代码
     * 或者实现了java.lang.Comparable接口的排序规则不适合当前的操作
     * 那么可以考虑使用Comparator的对象来排序
     *
     * 重写compare(Object o1,Object o2)方法，比较o1和o2的大小，如果方法返回正整数，则表示o1>o2
     * 如果返回负整数，表示o1<o2
     * 如果返回0，表示相等
     */
    @Test
    public void test3(){
        String[] arr=new String[]{"AA","CC","MM","GG","JJ","DD","KK"};
        Arrays.sort(arr, new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                if (o1 instanceof String && o2 instanceof String) {
                    String s1=(String)o1;
                    String s2=(String)o2;
                    return -s1.compareTo(s2);
                }
                throw new RuntimeException("输入的数据类型不一致");
            }
        });
        System.out.println(Arrays.toString(arr));
    }

    @Test
    public void test4(){
        Goods[] arr=new Goods[4];
        arr[0]=new Goods("lenovoMouse",34);
        arr[1]=new Goods("dellMouse",43);
        arr[2]=new Goods("xiaomiMouse",12);
        arr[3]=new Goods("huaweiMouse",65);

        Arrays.sort(arr, new Comparator() {
//指明商品比较大小的方式：按照产品名称从低到高排序，再按照价格从高到低排序
            @Override
            public int compare(Object o1, Object o2) {
                if (o1 instanceof Goods && o2 instanceof Goods) {
                    Goods g1=(Goods)o1;
                    Goods g2=(Goods)o2;
                    if (g1.getName().equals(g2.getName())) {
                        //如果名字相等，则按照价格进行排序
                        return -Double.compare(g1.getPrice(),g2.getPrice());
                    }else{
                        return g1.getName().compareTo(g2.getName());
                    }
                }
                throw new RuntimeException("输入数据类型错误！");
//                return 0;
            }
        });

        System.out.println(Arrays.toString(arr));
    }
    /**
     * Comparable接口与Comparator接口的使用对比
     * Comparable接口的方式一旦确定，保证Comparable接口实现类的对象在任何位置都可以比较大小
     * Comparator接口属于临时性的比较
     */

    /**
     * 其他常用的类
     * System/Math/
     * System:System的构造方法定义为私有的，所以不能够new对象，并且System内部定义的方法基本上都是静态方法，所以直接通过System
     * 进行调用，不需要通过对象使用
     * Math，Math类也提供了一系列的静态方法，所以直接通过Math进行调用，通常返回值为double
     *
     * Big Decimal  BigInteger
     * 如果需要表示比Long还要大的整数，不管是基本数据类型还是包装类都无能为力了，更不用说进行计算了
     *
     * java.math包的BigInteger可以表示不可变的任意精度的整数，BigInteger提供所有Java的基本整数操作符的对应物，并提供java.lang.Math
     * 的所有相关方法，另外，BigInteger还提供一下运算符：模运算，GCD运算，质数测试，素数生成，位操作以及一些其他操作
     *
     * 构造器：BigInteger(String val):根据字符串构建BigInteger对象
     *
     * BigDecimal对应的是float和double，可以提供更高的精度
     *
     */
    @Test
    public void test5(){
        BigInteger bigInteger = new BigInteger("12452345678545212356");
        BigDecimal bigDecimal = new BigDecimal("12345.345");
        BigDecimal bigDecimal1 = new BigDecimal("11");
        System.out.println(bigInteger);

        System.out.println(bigDecimal.divide(bigDecimal1,BigDecimal.ROUND_HALF_UP));//BigDecimal.ROUND_HALF_UP表示四舍五入
        System.out.println(bigDecimal.divide(bigDecimal1,15,BigDecimal.ROUND_HALF_UP));
        //表示保留小数点后15位

//        System.out.println(bigDecimal.divide(bigDecimal1));//没有指定BigDecimal.ROUND_HALF_UP四舍五入，如果不能除尽，则会报错，如果能够除尽，则不会报错
    }

}
```
