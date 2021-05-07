# Java8 Stream
+ 可以适用的场景
（1）从员工集合中筛选出salary大于8000的员工，并放置到新的集合中
（2）统计员工的最高薪资，平均薪资，薪资之和
（3）将员工的薪资从高到低排序，同样薪资者年龄小者在前
（4）将员工按性别分类，将员工按性别和地区分类，将员工按薪资是否高于8000分为两部分
stream将要处理的元素集合看作一种流，在流的过程中，借助stream api对流中的元素进行操作，比如，筛选，排序，聚合。
**stream可以由数组或集合创建，对流的操作可以分为两种**
（1）中间操作，每次返回一个新的流，可以有多个
（2）终端操作，每一个流都只能进行一次终端操作，终端操作结束以后流就无法再次使用，中断操作会返回一个新的集合或值
## 注意
（1）stream不存储数据，而是按照特定的规则对数据进行计算，一般会输出结果
（2）stream不会改变数据源，通常情况下会产生一个新的集合或一个值
（3）stream具有延迟执行特性，只有调用终端操作时，中间操作才会执行
## Stream可以通过集合数组创建
（1）通过java.util.Collection.stream()方法用集合创建流

# 应用
```
package com.atguigu.gulimall.search.thread.stine;


import org.junit.Test;

import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class StreamTest {
    /**
     * 创建流的方式
     * （1）通过集合创建stream：
     *         可以创建顺序流
     *         可以创建并行流
     */
    @Test
    public void test1(){
        //通过集合创建一个流
        List<String> list = Arrays.asList("a", "b", "c");
        //创建一个顺序流
        Stream<String> stream = list.stream();
        //创建一个并行流
        Stream<String> stringStream = list.parallelStream();

        //通过数组创建流   java.util.Arrays.stream(T[] array)
        int[] array={1,2,3,4};
        IntStream stream1 = Arrays.stream(array);

        //使用stream的静态方法  of(),iterator(),generate()  通过Stream的静态方法创建流
        Stream<Integer> integerStream = Stream.of(1, 2, 3, 4);
//        integerStream.forEach(System.out::println);

        Stream<Integer> limit = Stream.iterate(1, (x) -> x*x + 3).limit(5);//seed是作为x的初始值
//        limit.forEach(System.out::println);

        Stream<Double> limit1 = Stream.generate(Math::random).limit(3);
        limit1.forEach(System.out::println);

    }
    /**
     * 顺序流和并行流之间的区别
     * 顺序流Stream：由主线程按顺序对流执行操作。而parallelStream是并行流，内部以多线程并行执行的方式对流进行操作，但前提是流中的数据处理没有顺序要求
     * 如果流中的数据量足够大，并行流可以加快处理速度，除了直接创建并行流，还可以通过paralled()把顺序流转换为并行流
     *
     *Optional类是一个空以为null的容其对象，如果只存在则isPresent()方法会返回true，调用·get()方法会返回该对象
     * java8的Optional类
     */
    //把顺序流转为并行流
    @Test
    public void test2(){
        List<String> strings = Arrays.asList("1", "s", "12", "fd");
        strings.forEach(System.out::println);
        //转换
        System.out.println(strings.stream().isParallel());//不是并行流，输出false
        strings.parallelStream();
        Stream<String> parallel = strings.stream().parallel();

        System.out.println(parallel.isParallel());
    }

    @Test
    public void test3(){
        ArrayList<Person> personList = new ArrayList<Person>();
        personList.add(new Person("Tom", 8900, "male", "New York"));
        personList.add(new Person("Jack", 7000, "male", "Washington"));
        personList.add(new Person("Lily", 7800, "female", "Washington"));
        personList.add(new Person("Anni", 8200, "female", "New York"));
        personList.add(new Person("Owen", 9500, "male", "New York"));
        personList.add(new Person("Alisa", 7900, "female", "New York"));
        //通过集合在内存中存储数据代替数据库操作

        //遍历（foreach,find,match）
        /**
         * Stream也支持类似于集合的遍历操作和元素匹配，只是Stream中的元素是以Optional类型存在的
         */
        //筛选工资高于8000的人，并形成一个新的集合
        List<String> collect = personList.stream().filter(Person -> Person.getSalary() > 8000).map(Person::getName).collect(Collectors.toList());
        System.out.println("高于8000的员工的姓名："+collect);

        //获得员工工资最高的人
        Optional<Person> max = personList.stream().max(Comparator.comparingInt(Person::getSalary));
        System.out.println("获得员工工资最高的人："+max.get().getSalary());

        //将员工的工资全部加1000
        //(1)不改变原来员工集合的方式
        List<Person> collect1 = personList.stream().map(person -> {
            Person personNew = new Person(person.getName(), 0, 0, null, null);
            personNew.setSalary(person.getSalary() + 1000);
            return personNew;
        }).collect(Collectors.toList());

        System.out.println("一次改动前："+personList.get(0).getName()+"-->"+personList.get(0).getSalary());
        System.out.println("一次改动后："+collect1.get(0).getName()+"-->"+collect1.get(0).getSalary());

        //改变原来原来员工的方式
        List<Person> collect2 = personList.stream().map(person -> {
            person.setSalary(person.getSalary() + 1000);
            return person;
        }).collect(Collectors.toList());
        System.out.println("二次改动前："+personList.get(0).getName()+"-->"+personList.get(0).getSalary());
        System.out.println("二次改动后："+collect2.get(0).getName()+"-->"+collect2.get(0).getSalary());

        //归约的应用，求所有员工的工资之和和最高工资
        Integer reduce = personList.stream().reduce(0, (sum, p) -> sum += p.getSalary(), Integer::sum);
        Integer reduce1 = personList.stream().reduce(0, (sum, p) -> sum += p.getSalary(), (sum1, sum2) -> sum1 + sum2);
        Optional<Integer> reduce2 = personList.stream().map(Person::getSalary).reduce(Integer::sum);

        //求最高工资1
        Integer reduce3 = personList.stream().reduce(0, (max1, p) -> max1 > p.getSalary() ? max1 : p.getSalary(), Integer::max);
        //求最高工资2
        Integer reduce4 = personList.stream().reduce(0, (max2, p) -> max2 > p.getSalary() ? max2 : p.getSalary(), (max3, max4) -> max3 > max4 ? max3 : max4);
        System.out.println("工资之和："+reduce+","+reduce1+","+reduce2.get());
        System.out.println("最高工资："+reduce3+","+reduce4);
    }

    @Test
    public void test4(){
        List<Integer> list = Arrays.asList(7, 6, 8, 5, 4, 3);
        //遍历输出符合条件的元素   通过filter筛选
        list.stream().filter(x->x>6).forEach(System.out::println);
        //匹配第一个
        Optional<Integer> first = list.stream().filter(x -> x > 6).findFirst();
        //匹配任一个（适用于并行流）
        Optional<Integer> any = list.parallelStream().filter(x -> x > 6).findAny();
        //是否包含符合特定条件的元素
        boolean b = list.stream().anyMatch(x -> x < 6);
        System.out.println("匹配第一个值："+first.get());
        System.out.println("匹配任意一个值:"+any.get());
        System.out.println("是否存在大于6的值："+b);
    }

    //聚合操作，和数据库中的聚合函数max,min,avg,在聚合函数中传入Comparator接口
    @Test
    public void test5(){
        List<String> strings = Arrays.asList("adnm", "admmt", "pot", "xbangd", "weoujgsd");
        Optional<String> max = strings.stream().max(Comparator.comparing(String::length));
        System.out.println("最长的字符串："+max.get());

        //获取Integer集合中的最大值
        List<Integer> integers = Arrays.asList(7, 6, 9, 4, 11, 6);
        //自然排序
        Optional<Integer> max1 = integers.stream().max(Integer::compareTo);
        //自定义排序
        Optional<Integer> max2 = integers.stream().max(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return -o1.compareTo(o2);
            }
        });

        System.out.println("自然排序的最大值："+max1.get());
        System.out.println("自定义排序的最大值："+max2.get());

        //输出大于6的个数
        long count = integers.stream().filter(x -> x > 6).count();
        System.out.println(count);
    }

    //映射操作,可以将一个流的元素按照一定的映射规则映射到任一个流中，分为map和flatMap  ，映射有点像函数
    /**
     * (1)map:接受一个函数作为参数，该函数会被应用到每一个元素上，并将其映射成一个新的元素
     * （2）flatMap:接受一个函数作为参数，将流中的每个值都换成另一个流，然后把所有的流连接成一个流
     */
    @Test
    public void test6(){
        //将字符串全部改成大写，整数数组每个元素都加3
        String[] strArr={"abcd", "bcdd", "defde", "fTr"};
        List<String> collect = Arrays.stream(strArr).map(String::toUpperCase).collect(Collectors.toList());
        List<Integer> integers = Arrays.asList(1, 3, 5, 7, 9, 11);
        List<Integer> collect1 = integers.stream().map(x -> x + 3).collect(Collectors.toList());

        System.out.println("每个元素都大写："+collect);
        System.out.println("每个元素都加3："+collect1);

        //将两个字符数组合并成一个新的字符数组   将两个字符串合并成一个字符串
        List<String> strings = Arrays.asList("m,k,l,a", "1,3,5,7");
//        Iterator<String> iterator = strings.iterator();
//        while(iterator.hasNext()){
//            System.out.println(iterator.next());
//        }
        List<String> collect2 = strings.stream().flatMap(s -> {
            //将每一个元素转换成一个stream
            String[] split = s.split(",");
            Stream<String> s2 = Arrays.stream(split);
//flatMap接收一个函数作为参数，将流中的每一个值都转换成另一个流，然后把所有的流连成一个流，可以用于将两个字符数组的合并
            return s2;
        }).collect(Collectors.toList());
        System.out.println("处理前的集合："+strings);
        System.out.println("处理后的集合："+collect2);
    }

    /**
     * 归约：也称缩减，是把一个流缩减成一个值，能够实现对一个集合求和，求乘积和，和求最值操作
     *
     */
    @Test
    public void test7(){
        //求Integer集合的元素之和，乘积和最大值
        List<Integer> integers = Arrays.asList(1, 3, 2, 8, 11, 4);
        //求和方式1
        Optional<Integer> sum = integers.stream().reduce((x, y) -> x + y);//直接传入函数表达式
        //求和方式2
        Optional<Integer> reduce = integers.stream().reduce(Integer::sum);//直接传入函数名
        //求和方式三
        Integer reduce1 = integers.stream().reduce(0, Integer::sum);//直接将求和结果取出

        //求乘积
        Optional<Integer> reduce2 = integers.stream().reduce((x, y) -> x * y);

        //求最大值1
        Optional<Integer> reduce3 = integers.stream().reduce((x, y) -> x > y ? x : y);
        //求最大值2
        Integer reduce4 = integers.stream().reduce(1, Integer::max);

        System.out.println("intgers求和："+sum.get()+","+reduce.get()+","+reduce1);
        System.out.println("integers求乘积："+reduce2.get());
        System.out.println("integers求最大值："+reduce3.get()+","+reduce4);
    }

    /**
     * 收集：collect：收集，可以说是内容最多的，功能最丰富的部分了，就是把一个流收集起来，最终可以收集成一个值，也可以收集成一个集合
     * collect主要依赖java.util.stream.Collectors类内部的静态方法
     * （1）归集（toSet,toList,toMap）,
     * 因为流不存储数据，那么在流中的数据完成处理后，需要将流中的数据重新归集到新的集合中，toList,toSet,toMap比较常用，另外还有toCollection,toConcurrentMap等复杂用法
     *
     * （2）统计   求所有的统计信息
     *
     * （3）分组   分区
     *
     * （4）接合
     *
     * （5）归约（reducing）：Collectors类提供的reducing方法，相比于stream本身的reducing方法，增加了对自定义的支持
     *
     * （6）排序
     *
     * （7）提取、组合
     *
     */
    @Test
    public void test8(){
        //归集    归于不同的集合
        List<Integer> integers = Arrays.asList(1, 6, 3, 4, 6, 7, 9, 6, 20);
        List<Integer> collect = integers.stream().filter(x -> x % 2 == 0).collect(Collectors.toList());
        Set<Integer> collect1 = integers.stream().filter(x -> x % 2 == 0).collect(Collectors.toSet());
        ArrayList<Person> personList = new ArrayList<>();
        personList.add(new Person("Tom", 8900, 23, "male", "New York"));
        personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
        personList.add(new Person("Lily", 7800, 21, "female", "Washington"));
        personList.add(new Person("Anni", 8200, 24, "female", "New York"));
        Map<?, Person> collect2 = personList.stream().filter(p -> p.getSalary() > 8000).collect(Collectors.toMap(Person::getName, p -> p));
        System.out.println("toList:"+collect);
        System.out.println("toSet:"+collect1);
        System.out.println("toMap"+collect2);
    }

    //统计
    @Test
    public void test9(){
        /**
         * Collectors提供了一系列用于数据统计的静态方法
         * 计数：count;平均值：averagingInt,averagingLongs,averagingDouble
         * 最值：maxBy,minBy
         * 求和：summingInt,summingLong,summingDouble
         * 统计以上所有：summarizingInt summarizingLong,summarizingDouble
         */
        ArrayList<Person> personList = new ArrayList<>();
        personList.add(new Person("Tom", 8900, 23, "male", "New York"));
        personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
        personList.add(new Person("Lily", 7800, 21, "female", "Washington"));
        //求总和
        Long collect = personList.stream().collect(Collectors.counting());
        //求最高工资
        Optional<Integer> max = personList.stream().map(Person::getSalary).collect(Collectors.maxBy(Integer::compare));
        //求平均工资
        Double collect1 = personList.stream().collect(Collectors.averagingDouble(Person::getSalary));
        //求工资之和
        Integer collect2 = personList.stream().collect(Collectors.summingInt(Person::getSalary));
        //一次性统计所有信息
        DoubleSummaryStatistics collect3 = personList.stream().collect(Collectors.summarizingDouble(Person::getSalary));

        System.out.println("员工总数："+collect);
        System.out.println("员工平均工资："+collect1);
        System.out.println("员工工资总和"+collect2);
        System.out.println("员工工资所有统计："+collect3);
    }

    /**
     * 分组：将集合分为多个map，比如员工按性别分组，有单级分组和多级分组       分组分为多个map  ，多个部分
     * 分区：将stream按条件分为两个map，比如员工按薪资是否高于8000分为两部分  分区只能分为两个部分
     */
    @Test
    public void test10(){
        List<Person> personList = new ArrayList<Person>();
        personList.add(new Person("Tom", 8900, "male", "New York"));
        personList.add(new Person("Jack", 7000, "male", "Washington"));
        personList.add(new Person("Lily", 7800, "female", "Washington"));
        personList.add(new Person("Anni", 8200, "female", "New York"));
        personList.add(new Person("Owen", 9500, "male", "New York"));
        personList.add(new Person("Alisa", 7900, "female", "New York"));

        //将员工工资是否高于8000分为两部分
        Map<Boolean, List<Person>> collect = personList.stream().collect(Collectors.partitioningBy(x -> x.getSalary() > 8000));
        //将员工按性别分组
        Map<String, List<Person>> collect1 = personList.stream().collect(Collectors.groupingBy(Person::getSex));

        //将员工先按性别分组，再按地区分组
        Map<String, Map<String, List<Person>>> collect2 = personList.stream().collect(Collectors.groupingBy(Person::getSex, Collectors.groupingBy(Person::getArea)));

        System.out.println("员工按薪资是否大于8000分组情况："+collect);
        System.out.println("员工按性别分组情况："+collect1);
        System.out.println("员工按性别，地区分组"+collect2);

    }

    /**
     * 接合  joining可以将stream中的元素用特定的连接符（没有的话，直接连接）连接成一个字符串
     */
    @Test
    public void test11(){
        List<Person> personList = new ArrayList<Person>();
        personList.add(new Person("Tom", 8900, 23, "male", "New York"));
        personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
        personList.add(new Person("Lily", 7800, 21, "female", "Washington"));

        String collect = personList.stream().map(p -> p.getName()).collect(Collectors.joining(","));
        System.out.println("所有员工的姓名："+collect);
        List<String> strings = Arrays.asList("a", "b", "c");
        String collect1 = strings.stream().collect(Collectors.joining("-"));
        System.out.println("拼接以后的字符串："+collect1);

    }

    /**
     * 归约
     */
    @Test
    public void test12(){
        List<Person> personList = new ArrayList<Person>();
        personList.add(new Person("Tom", 8900, 23, "male", "New York"));
        personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
        personList.add(new Person("Lily", 7800, 21, "female", "Washington"));

        //每个员工减去起征点后的薪资之和
        Integer collect = personList.stream().collect(Collectors.reducing(0, Person::getSalary, (i, j) -> (i + j - 5000)));
        System.out.println("员工扣税薪资之和"+collect);
        Optional<Integer> reduce = personList.stream().map(Person::getSalary).reduce(Integer::sum);
        System.out.println("员工薪资总和："+reduce.get());
    }

    /**
     * 排序,中间操作：有两种排序
     * （1）sorted:自然排序，流元素需要实现Comparable接口
     * （2）sorted（Comparator com）:Comparator排序器自定义排序
     */
    @Test
    public void test13(){
        //将员工工资由高到低（工资一样则按年龄有大到小排序）
        List<Person> personList = new ArrayList<Person>();

        personList.add(new Person("Sherry", 9000, 24, "female", "New York"));
        personList.add(new Person("Tom", 8900, 22, "male", "Washington"));
        personList.add(new Person("Jack", 9000, 25, "male", "Washington"));
        personList.add(new Person("Lily", 8800, 26, "male", "New York"));
        personList.add(new Person("Alisa", 9000, 26, "female", "New York"));

        //按工资升序排序（自然排序）
        List<String> collect = personList.stream().sorted(Comparator.comparing(Person::getSalary)).map(Person::getName).collect(Collectors.toList());

        //按工资倒序排序
        List<String> collect1 = personList.stream().sorted(Comparator.comparing(Person::getSalary).reversed()).map(Person::getName).collect(Collectors.toList());

        //先按工资再按年龄升序排序
        List<String> collect2 = personList.stream().sorted(Comparator.comparing(Person::getSalary).thenComparing(Person::getAge)).map(Person::getName).collect(Collectors.toList());

        //先按工资再按年龄自定义排序(降序)
        List<String> collect3 = personList.stream().sorted((p1, p2) -> {
            if (p1.getSalary() == p2.getSalary()) {
                return p2.getAge() - p1.getAge();
            } else {
                return p2.getSalary() - p1.getSalary();
            }
        }).map(Person::getName).collect(Collectors.toList());

        System.out.println("按工资升序排序"+collect);
        System.out.println("按工资倒序排序："+collect1);
        System.out.println("按工资再按年龄升序排序："+collect2);
        System.out.println("先按工资再按年龄自定义排序："+collect3);
    }

    /**
     * 提取，组合   流也可进行合并，去冲个，限制，跳过等操作
     */
    @Test
    public void test14(){
        String[] arr1={"a","b","c","d"};
        String[] arr2={"d","e","f","g"};

        Stream<String> arr11 = Stream.of(arr1);
        Stream<String> arr21 = Stream.of(arr2);
        //合并两个流，去重
        List<String> collect = Stream.concat(arr11, arr21).distinct().collect(Collectors.toList());
        //limit限制从流中获得前n个数据
        List<Integer> collect1 = Stream.iterate(1, x -> x + 2).limit(10).collect(Collectors.toList());
        //skip跳过前n个数据
        List<Integer> collect2 = Stream.iterate(1, x -> x + 2).skip(1).limit(5).collect(Collectors.toList());


        System.out.println("合并流："+collect);
        System.out.println("limit:"+collect1);
        System.out.println("skip:"+collect2);
    }
}
```
