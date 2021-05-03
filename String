## String的不可变性-实例化方式-字符串的拼接
```
package com.atguigu.gulimall.search.thread.String1;

import org.junit.Test;

/**
 *String的使用
 * （1）String字符串，使用一对""引起来表示
 * （2）String声明为final类型，是不可被继承的
 * （3）String实现了Serializable接口：表示字符串时支持序列化的，
 * （4）String实现了Comparable接口，是可以按照字典序比较大小的
 * （5）String内部定义了final char[] value用于存储字符串数据
 * （6）String代表一个不可变的字符序列，简称：不可变性
 * 体现：1、当对字符串重新赋值时，需要重新指定内存区域赋值，不能使用原有的value进行赋值
 * 2、当对现有的字符串进行连接操作时，也需要重新制定内存区域赋值，不能使用原有的value赋值
 *3、当调用String的replace()方法修改字符或字符串时，也必须重新指定内存创建
 */
public class StringTest {
    /**
     * String的不可变性
     */
    @Test
    public void test1(){
        String s1="abc";   //直接赋值的字面量 ，String是唯一一个可以直接复制的引用数据类型
        String s2="abc";  //通过字面量的方式给一个字符串赋值，此时的字符串值声明在字符串常量池中
        //常量池是不会存两个相同的字符串
        //实际上s1和s2使用的是同一个值，直接通过s1==s2比较两者的地址值会发现是true
        System.out.println(s1==s2);//true

        s1="hello";    //此时在常量池中新创建一个字符串
        System.out.println(s1);  //hello
        System.out.println(s2);  //abc ,修改s1的值，只会在常量池中新创建一个字符串，s2的值不会发生变化

        System.out.println("==================================");

        String s3="abc";
        s3+="def";
        System.out.println(s3);//abcdef   此时s3也是在字符串常量池中新创建一个abcdef,不能在原有的位置进行修改，体现了不可变性
        System.out.println(s2);

        System.out.println("==================================");
        String s4="abc";
        String s5 = s4.replace('a', 'm');
        System.out.println(s4);//abc
        System.out.println(s5);//mbc
    }

    /**
     * String的实例化方式
     * 方式一：通过字面量定义的方式
     * 方式二：通过new+构造器的方式
     *面试题：String s=new String("abc");方式创建对象，在内存中创建流几个对象
     * 两个：一个是对空间中new结构；另一个是char[]对应的常量池中的数据"abc",如果常量池中已经有了abc，则使用已经存在的abc，因为常量池中
     * 不会出现两个abc
     *
     */
    @Test
    public void test2(){
        String s2="javaEE";
        String s1="javaEE";//此时的s1,s2的数据javaEE声明在方法区的字符串常量池中
    //通过new的方式，此时的s3和s4保存的地址值，是数据在堆空间中开辟空间以后对应的地址值
        String s3=new String("javaEE");
        String s4=new String("javaEE");
        System.out.println(s1==s2);//true
        System.out.println(s1==s3);//false
        System.out.println(s1==s4);//false
        System.out.println(s3==s4);//false

        System.out.println("=====================================================");
        Person p1 = new Person("Tom", 12);
       Person p2 = new Person("Tom",12);
        System.out.println(p1.name.equals(p2.name));//true
        System.out.println(p1.name==p2.name);//true

        p1.name="Jerry";
        System.out.println(p2.name);//Tom  体现不可变性
    }

    /**
     * 字符串的拼接方式:
     * (1)常量与常量的拼接结果在常量池，且常量池中不会存在相同内容的常量
     * （2）只要其中有一个是变量(s1和s2)，结果就在堆中
     * （3）如果拼接的结果调用intern()方法，返回值就在常量池中
     *
     */
    @Test
    public void test3(){
        String s1="javaEE";
        String s2="hadoop";
        String s3="javaEEhadoop";
        String s4="javaEE"+"hadoop";
        String s5=s1+"hadoop";
        String s6="javaEE"+s2;
        String s7=s1+s2;
        String s8=(s1+s2).intern();//此时返回值就是在常量池中

        System.out.println(s3==s4);//true
        System.out.println(s3==s5);//false
        System.out.println(s3==s6);//false
        System.out.println(s5==s6);//false
        System.out.println(s3==s7);//false
        System.out.println(s5==s7);//false
        System.out.println(s6==s7);//false

        System.out.println(s3==s8);  //true
        System.out.println(s4==s8);   //true

    }

    /**
     * 加上final的变量相当于常量，此时进行字符串拼接，仍然是按照常量进行操作
     */
    @Test
    public void test4(){
        String s1="javaEEhadoop";
        String s2="javaEE";
        String s3=s2+"hadoop";
        System.out.println(s1==s3);//false

        final String s4="javaEE";
        String s5=s4+"hadoop";
        System.out.println(s1==s5);//true   此时s4作为常量，所以和hadoop在常量池中进行拼接，所以返回地址相同
    }

}
```
# String与基本数据类型，包装类和char[] byte[]之间的转换
```
package com.atguigu.gulimall.search.thread.String1;

import org.junit.Test;

import java.io.UnsupportedEncodingException;
import java.util.Arrays;

/**
 * 涉及到String类与其他结构之间的转换
 */
public class StringTest1 {

    /**
     * String与基本数据类型、包装类之间的转换
     * String-->基本数据类型，包装类   调用包装类的静态方法ParseXXx（str）
     *将基本数据类型/包装类---->String：调用String重载的valueOf(xxx)
     */

    @Test
    public void test1(){
        String str1="123";
//        int num=(int)str1;//错误，只有子父类的关系的才能够强转
        int i = Integer.parseInt(str1);
        System.out.println(i);
        String s = String.valueOf(i);
        System.out.println(s);
        //将基本数据类型转为字符串的方式二：
        String str2=i+"";
        System.out.println(str2);
    }

    /**
     * String与char[]之间的转换
     * String->char[]  调用String的toCharArray()即可
     *char[] ->String   调用String的构造器
     */
    @Test
    public void test2(){
        String str1="abc123";
        //String转为char[]
        char[] chars = str1.toCharArray();
        System.out.println(chars);
        for(int i=0;i<chars.length;i++){
            System.out.println(chars[i]);
        }
        //char[]->String：直接调用String的构造器即可
        char[] chars1 = {'h', 'e', 'l', 'l', 'o'};
        String s = new String(chars1);
        System.out.println(s);

    }

    /**
     * String和byte[]之间的转换
     * 编码：String->byte[] :调用String的getBytes()
     *
     * 解码：byte[]->String：调用String的构造器
     *
     * 说明：解码时，要求解码使用的字符集必须与编码时使用的字符集一直，否则会出现乱码
     *
     */
    @Test
    public void test3() throws UnsupportedEncodingException {
        String str1="abc123中国";
        byte[] bytes = str1.getBytes();//使用默认的字符集进行转换
        System.out.println(Arrays.toString(bytes));

        //使用指定的字符集进行转换getBytes(charset),会出现没有指定编码集的异常
        byte[] gbks = str1.getBytes("gbk");//utf-8中一个汉字占用3个字节，gbk中一个汉字占用2个字节

        System.out.println(Arrays.toString(gbks));

        System.out.println("===============================");

        String s = new String(bytes);//使用默认的字符集进行解码
        System.out.println(s);

        //使用指定的字符集进行解码
        String gbk = new String(gbks, "gbk");
        System.out.println(gbk);

    }
}
```
# String的常用27个方法
```
package com.atguigu.gulimall.search.thread.String1;

import org.junit.Test;

/**
 *(1)int length():返回字符串的长度：return value.length()
 * (2)char charAt(int index):返回某索引处的字符return value[index]
 * (3)boolean isEmpty():判断是否是空字符串:return value.length==0
 * (4)String toLowerCase():使用默认语言环境，将String中的所有字符转换为小写
 * （5）String toUpperCase():使用默认语言环境，将String中的所有字符转换为大写
 * （6）String trim()：返回字符串的副本，忽略前导空白和尾部空白
 * （7）boolean equals(Object obj):比较字符串的内容是否相同
 * （8）boolean equalsIgnoreCase(String anotherString):与equals()类似，忽略大小写
 * （9）String concat(String str):将指定字符串连接到此字符串的结尾，等价于用"+"
 * （10）int compareTo(String anotherString):比较两个字符串的大小
 * （11）String substring(int beginIndex):返回一个新的字符串，它是此字符串的beginIndex开始)
 * （12）String subString(int beginIndex,int endIndex):返回一个新字符串，它是此字符串从beginIndex
 *
 * (13)boolean endsWith(String suffix):测试指定的字符串是否以指定的字符串结束
 * （14）boolean startsWith(String prefix):测试此字符串是否以指定的前缀开始
 * （15）boolean startsWith(String prefix,int toffset):测试此字符串从指定索引的子字符串是否以指定前缀开始
 *
 * (16)boolean contains(charSequence s):当且仅当此字符串包含指定的char值序列时，返回true)
 * (17)int indexOf(String str):返回指定字符串在此字符串中第一次出现的索引  ，如果模式串中没有子字符串，则返回-1
 * (18)int indexOf(String str,int fromIndex):从模式串的指定位置fromIndex位置开始找，可以应用在查找指定字符串在模式串中出现过几次
 * (19)int lastIndexOf(Strings tr): 从后往前找
 * (20)int lastIndexOf(String str,int fromIndex):
 *注：index()和lastIndexOf()如果没找到都是返回-1
 *
 * （21）String replace(char oldChar,char newChar):返回一个新的字符串它是通过用newChar替换
 * （22）String replace(charSequence target,CharSequence replacement):使用指定的字面值替换序列
 * （23）String replaceAll(String regex,String replacement):使用给定的replacement替换次
 * （24）String replaceFirst(String regex,String replacement):使用给定的replacement替换此
 *regex正则表达式
 * （25）boolean matches(String regex):告知此字符串是否匹配给定的正则表达式
 * （26）String[] split(String regex):
 * (27)String[] split(String regex,int limit):根据匹配给定的正则表达式
 */

public class StringMethodTest {

    @Test
    public void test1(){
        String s1="helloworld";
        System.out.println(s1.length());
        System.out.println(s1.charAt(2));
        System.out.println(s1.isEmpty());
        String s2="";
        System.out.println(s2.isEmpty());

        String s = s1.toUpperCase();
        System.out.println(s);
        System.out.println(s1);//将字符串转为大写，是带返回值的，所以结果都保存在返回值中，原来的字符串s1没有改变

        //同样的toLowerCase()
        String s3 = s.toLowerCase();
        System.out.println(s3);
        System.out.println(s);

        System.out.println("+++====================================");

        String s4="   as  dff  gh fh ";
        String trim = s4.trim();
        System.out.println("-----"+trim+"-----");//去除了首部和尾部的空白
        System.out.println("-----"+s4+"-----");

        System.out.println("+++====================================");
        String s5="HelloWorld";
        String s6="helloworld";
        System.out.println(s5.equals(s6));
        System.out.println(s5.equalsIgnoreCase(s6));
    }
    @Test
    public void test2(){
        String s1="abc";
        String s2=s1.concat("dfe");
        System.out.println(s2);
        //比较大小
        String s3="abc";
        String s4=new String("ads");//通过对应的ascll
        System.out.println(s3.compareTo(s4));

        System.out.println("+========================================");
        String s7="liuleigoldstine";
        String substring = s7.substring(6, 10);
        System.out.println(substring);
    }
    
    @Test
    public void test3(){
        String str1="helloworld";
        boolean ld = str1.endsWith("ld");
        System.out.println(ld);

        boolean he = str1.startsWith("He");
        System.out.println(he); //区分大小写

//        指定str1从2开始是否是该字符串
        boolean ll = str1.startsWith("ll", 2);
        System.out.println(ll);

        String str2="Wo";//区分大小写
        System.out.println(str1.contains(str2));

        System.out.println(str1.indexOf("lo"));//找到子字符串在模式串中出现的位置

        System.out.println(str1.indexOf("lo",5));//指定从模式串中5的位置开始向后面找 没有找到，返回-1

        System.out.println(str1.lastIndexOf("or"));//从后往前找，但是返回的索引还是从前往后的索引
        System.out.println(str1.lastIndexOf("l",5));//从指定位置fromIndex从后往前找
        //什么情况下，indexOf()和lastIndexOf()的查找结果一样
        //（1）只有一个对应的串在模式串中，（2）模式串中没有匹配的字符串，返回-1
    }

    @Test
    public void test4(){
         String str1="goldstine";
        String replace = str1.replace('s', 'l');
        System.out.println(replace);
        System.out.println(str1);
    }
}
```
# String-StringBuffer-StringBuilder
```
package com.atguigu.gulimall.search.thread.String1;

import org.junit.Test;

/**
 *String/StringBuffer/StringBuilder之间的异同
 * String:不可变的字符序列  jdk1.0
 * StringBuffer:可变的字符序列 jdk1.0
 * StringBuilder：可变的字符序列  jdk1.5 由于StringBuffer效率比较低（因为StringBuffer是线程安全的），所以在jdk1.5提供了线程不安全但是效率高的StringBuilder
 * 在jdk1.0的时候的线程安全的类，在jdk1.5基本上都提供了线程不安全但效率高的类
 *StringBuilder与StringBuffer的方法基本上都是一样的，区别在于没有synchronized了
 *
 * 如果时多线程问题，就是用线程安全的StringBuffer；如果不涉及线程安全性，则使用StringBuilder
 * 相同点就是都是与字符串相关的类；底层都是char[] value存储
 *
 * 源码分析：
 * String str=new String();//char[] value=new char[0];
 * String str1=new String("abc");//char[] value=new char[]{'a','b','c'}
 *
 * StringBuffer sb1=new StringBuffer();//char[] value=new char[16];底层创建一个长度为16的字符数组
 * String.out.println(sb1.length());//0  注：这里sb1.length()实际上在底层输出的时数组里面的值的个数，也就是append()的计数值个数
 * sb1.append('a');//value[0]='a';
 * sb1.append('b');//value[1]='b';
 *
 * StringBuffer sb2=new StringBuffer("abc");char[] value=new char["abc".length()+16] 创建了一个"abc".length()+16长度的字符数组
 * //问题1：System.out.println(sb2.length());  //这里返回的还是value[]中的元素的个数 3
 * //扩容问题：如果要添加的数据底层数组放不下了，那就需要扩容底层数组
 *默认情况下，扩容为原来容量的2倍+2，同时将原有数组中的元素复制到新的数组中
 *
 * private int newCapacity(int minCapacity) {
 *         // overflow-conscious code
 *         int newCapacity = (value.length << 1) + 2;
 *         if (newCapacity - minCapacity < 0) {
 *             newCapacity = minCapacity;
 *         }
 *         return (newCapacity <= 0 || MAX_ARRAY_SIZE - newCapacity < 0)
 *             ? hugeCapacity(minCapacity)
 *             : newCapacity;
 *     }
 *
 *    指导建议：如果是频繁可变字符串的操作，建议不要使用String
 *    也要尽量避免StringBuffer的扩容情况
 *    所以可以使用StringBuffer(int capacity);构造方法指定容量大小
 *    StringBuilder(int capacity),StringBuilder的扩容及之与StringBuffer相同
 *
 *    String/StringBuffer/StringBuilder的常用方法
 *StringBuffer为例：StringBuilder是一样的
 *    (1)StringBuffer append(xxx):提供了很多的append()方法，用于进行字符串的拼接
 *    （2）StringBuffer delete(int start,int end):删除指定位置的内容[)
 *    （3）StringBuffer replace(int start,int end,String str):把[start,end]位置替换为str
 *    (4)StringBuffer insert(int offset,xxx):在指定位置插入xxx
 *    (5)StringBuffer reverse():把当前字符序列进行逆转
 *(6)public int indexOf(String str):返回当前字符串在模式串中的位置
 * （7）public String subString(int start,int end)
 * (8)public int length()
 * (9)public char charAt(int n)
 * (10)public void setCharAt(int n,char ch)
 *
 * 总结：
 * 增：append()   方法链的使用   str.qppend(xxx).append(yyy).append(zzz)
 * 删：delete(int start,int end)
 * 改：replace(int start,int end,String str)/setCharAt(int n,char ch)
 * 查：charAt(int index)
 * 插：insert(int index,xxx)
 * 长度：length()
 * 遍历：for+charAt()  或者toString()
 *
 * 效率：String<StringBuffer<StringBuilder
 *
 */

public class StringBufferBuilderTest {
    
    @Test
    public void test1() {
        StringBuffer abc = new StringBuffer("abc");
        abc.setCharAt(0,'m');//没有返回值，所以是可变的，直接在原来的字符串常量池"abc"的基础上进行修改
        System.out.println(abc);

        StringBuffer stringBuffer = new StringBuffer();
        System.out.println(stringBuffer.length());//0
    }

    @Test
    public void test2(){
        //StringBuffer的常用方法
        StringBuffer abc = new StringBuffer("abc");
        abc.append(1);
        abc.append('1');
        System.out.println(abc);

//        StringBuffer delete = abc.delete(2, 4);//这里是可变的，但是返回了一个新的StringBuffer 返回的新的StringBuffer
//        System.out.println(abc);
//        System.out.println(delete);

        //把c1换成helllo
        StringBuffer hello = abc.replace(2, 4, "hello");//这里也有返回值，但是是可变的
        System.out.println(hello);
        System.out.println(abc);

        StringBuffer insert = abc.insert(2, false);
        System.out.println(insert);
        System.out.println(abc);

        System.out.println(abc.length());

        StringBuffer reverse = abc.reverse();
        System.out.println(reverse);
        System.out.println(abc);

        String substring = abc.substring(1, 3);//注：此时就需要返回值，获取字串后，原来的模式串不会发生变化
        System.out.println(substring);//ol
        System.out.println(abc);//1olleheslafba
    }
}
```
