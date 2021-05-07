## java的日期时间的使用
```
package com.atguigu.gulimall.search.thread.Date;

import org.junit.Test;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.time.*;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.time.temporal.TemporalAccessor;
import java.util.Calendar;
import java.util.Date;

/**
 * JDK8之前日期和时间的api测试
 * System.currentTimeMills()
 * java.util.Date  java.sql.Date
 * SimpleDateFormat
 * Calendar
 *
 */
public class DateTimeTest {

    /**
     * System类中的currentTimeMills()
     * 返回当前时间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差
     */
    @Test
    public void test1(){
        long l = System.currentTimeMillis();
        System.out.println(l);
    }

    /**
     * java.util.Date类
     *       |---java.sql.Date类
     *
     *     (1)两个构造器的使用
     *     （2）两个方法的使用:toString():显示当前的年月日，时分秒
     *          getTime():获取当前对象对应的毫秒数，时间戳
     *  java.sql.Date:对应着数据库中的日期类型的变量
     *      （1）如何实例化
     *      (2)如何将sql.Date对象转换为util.Date对象      直接赋值
     *      （3）如何将util.Date对象转换为sql.Date对象
     *
     */
    @Test
    public void test2(){
        //构造器一：Date()空参构造器，相当于创建一个当前时间的Date对象
        Date date1 = new Date();
        String s = date1.toString();
        System.out.println(s);
        System.out.println(date1.getTime());

        //构造器二:创建指定毫秒数的date对象
        Date date2 = new Date(1619880389843L);
        System.out.println(date2.toString());

        //创建java.sql.Date对象
        java.sql.Date date3 = new java.sql.Date(1619880909493L);
        System.out.println(date3.toString());
        System.out.println(date3.getTime());

        //将util.Date对象转为sql.Date对象
//        java.util.Date date4 = new java.sql.Date(1619880909493L);//将子类对象赋给父类对象
//        java.sql.Date date = (java.sql.Date) date4;  //多态

        Date date6=new Date();
        //java.sql.Date date61 = (java.sql.Date) date6;  //编译不报错，但是运行时报错 new的时父类，不可能强转为子类对象

        java.sql.Date date7=new java.sql.Date(date6.getTime());
    }

    /**
     * jdk8之前的时间和日期api测试
     *      java.text.SimpleDateFormat类:对日期Date类的格式化和解析
     * Date类的api不易于国际化，大部分被废弃了，java.text.SimpleDateFormat类是一个不与语言环境有关的方式来格式化与解析日期的具体类
     *
     * 1、两个操作
     * （1）格式化：日期---》字符串
     * （2）解析：格式化的逆过程，字符串---》日期
     *2.simpleDateFormat实例化
     */
    @Test
    public void test3() throws ParseException {
        //实例化SimpleDateFormat,  使用默认的构造器
        SimpleDateFormat sdf = new SimpleDateFormat();
        Date date = new Date();
        //将日期date格式化
        String format = sdf.format(date);
        System.out.println(format);
        //将日期进行解析，但是传入的字符串的格式需要是   21-5-1 下午11:21

        String s="21-5-1 下午11:21";
        Date date1 = sdf.parse(s);
        System.out.println(date1);

        //***********************************************
//        SimpleDateFormat sdf1 = new SimpleDateFormat("yyyyy.MMMMM.dd GGG hh:mm aaa");
        SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
        //按照指定方式格式化，调用带参数的构造器
        String format1 = sdf1.format(date);
        System.out.println(format1);
        //解析
        Date parse = sdf1.parse("2021-05-01 11:29:26");
        System.out.println(parse);

    }
    /**
     * 抽象类calendar的使用:
     *  获取Calendar实例的方法
     *  （1）使用Calendar.getInstance()方法
     *  （2）调用它的子类GregorianCalendar的构造器
     *
     * 获取月份时：1月是0，就是从0开始计数的
     * 获取星期时：周日是1
     */
    @Test
    public void test4(){
        //方式一，调用静态方法
        Calendar instance = Calendar.getInstance();//其实也是通过Calendar的子类new的对象，只不过赋给了Calendar

        //常用方法
        //get
        int i = instance.get(Calendar.DAY_OF_MONTH);
        System.out.println(i);

        //set
        instance.set(Calendar.DAY_OF_MONTH,23);
        System.out.println(instance.get(Calendar.DAY_OF_MONTH));
        //add
        instance.add(Calendar.DAY_OF_MONTH,3);   //如果要减去几天，直接将3改为-3即可
        System.out.println(instance.get(Calendar.DAY_OF_MONTH));

        //getTime   将日历类Calendar转为Date对象
        Date time = instance.getTime();
        System.out.println(time);

        //setTime    将Date对象设置为日历类Calendar
        Date date = new Date();
        instance.setTime(date);
        System.out.println(instance.get(Calendar.DAY_OF_MONTH));

    }

    /**
     * （1）Calendar是可变的，所以用的也比较少
     * jdk8中的日期api
     * （2）Calendar的偏移性，问题，年的偏移量1900加上，月的偏移量为1
     * （3）格式化：格式化只针对Date，Calendar不能够格式化
     * （4）他们不是线程安全的，不能处理闰秒
     * 总结：对时间和日期的处理一直时程序员最痛苦的事
     * 第三次引入API是成功的，并且java8中引入的java.time.API已经纠正了过去的缺陷，
     * java8吸收了joda-time的精华
     */
    @Test
    public void test5(){
        /**
         * LocalDate  LocalTime/LocalDateTime
         */

        /**
         * (1)实例化：now():获取当前的日期，时间，日期+时间
         * LocalDateTime相对与另外两个使用频率相对更高
         */

        LocalDate now = LocalDate.now();
        LocalTime now1 = LocalTime.now();
        LocalDateTime now2 = LocalDateTime.now();

        System.out.println(now);
        System.out.println(now1);
        System.out.println(now2);

        //of():设定指定的年月日时分秒，没有偏移量的
        LocalDateTime localDateTime1 = LocalDateTime.of(2021, 5, 2, 11, 51, 43);
        System.out.println(localDateTime1);

        //getXXX()
        System.out.println(localDateTime1.getDayOfMonth());
        System.out.println(localDateTime1.getDayOfWeek());
        System.out.println(localDateTime1.getDayOfYear());
        System.out.println(localDateTime1.getHour());

        //体现不可变性，原来的对象是不可以变的，但是可以通过设置返回一个新的对象，新的对象是设置的变化
        LocalDate localDate1 = now.withDayOfMonth(22);
        System.out.println(localDate1);
        System.out.println(now);
        //withXXX()相当于设置相关属性
        LocalDateTime localDateTime = localDateTime1.withHour(4);
        System.out.println(localDateTime);
        System.out.println(localDateTime1);

        //plusXXX()  加  minusXXX()减
        LocalDateTime localDateTime2 = localDateTime.plusMonths(3);
        System.out.println(localDateTime2);
        System.out.println(localDateTime);

    }

    /**
     * 瞬时：Instant
     *实例化:now():获取本初子午线对应的时间
     * atOffset():根据时区获得时间
     */
    @Test
    public void test6(){
        Instant now = Instant.now();
        System.out.println(now);

        OffsetDateTime offsetDateTime = now.atOffset(ZoneOffset.ofHours(8));
        System.out.println(offsetDateTime);

        //获取顺势点对应的毫秒数
        long l = now.toEpochMilli();
        System.out.println(l);

        //设置瞬时点对应的毫秒数ofEpochMilli():通过给定的毫秒数，获取Instant实例
        Instant instant = Instant.ofEpochMilli(1619928644261L);
        System.out.println(instant);

    }

    /**
     * DateTimeFormatter:格式化或解析日期、时间
     * 类似于SimpleDateFormat
     */
    @Test
    public void test7(){
        //实例化
        /**
         * 方式一：预定义的标准格式
         * 方式二：本地化相关的格斯
         * 方式三：自定义格式
         */
//方式一：
        DateTimeFormatter formatter = DateTimeFormatter.ISO_LOCAL_DATE_TIME;
        System.out.println(formatter);
        LocalDateTime now = LocalDateTime.now();
        //格式化
        String format = formatter.format(now);
        System.out.println(now);//2021-05-02T12:27:13.797
        System.out.println(format);//2021-05-02T12:27:13.797
        //解析
        TemporalAccessor parse = formatter.parse("2021-05-02T12:21:26.737");
        System.out.println(parse);
//方式二
        //本地化相关格式 ，如：ofLocalizedDateTime()
        //FormatStyle.LONG/FormatStyle.MEDIUM/FormatStyle.SHORT:适用于LocalDateTime
        DateTimeFormatter formatter1 = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT);
        String format1 = formatter1.format(now);
        System.out.println(format1);//21-5-2 下午12:27
        System.out.println(now);//2021-05-02T12:27:13.797

        //本地化相关的格式，如ofLocalizedDate()
        //FormatStyle.FULL/FormatStyle.LONG/FormatStyle.MEDIUM/FormatStyle.SHORT:适用于LocalDate
        DateTimeFormatter formatter2 = DateTimeFormatter.ofLocalizedDate(FormatStyle.FULL);
        String format2 = formatter2.format(LocalDate.now());
        System.out.println(format2);

        //方式三
        //ofPattern("yyyy-MM-dd hh:mm:ss ")
        DateTimeFormatter formatter3 = DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss");
        String format3 = formatter3.format(LocalDateTime.now());
        System.out.println(format3);
        //解析
        TemporalAccessor parse1 = formatter3.parse("2021-05-02 12:37:35");
        System.out.println(parse1);

    }

}
```
