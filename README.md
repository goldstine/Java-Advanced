# 集合类Collection
+ 常用方法：
add(),addAll(),isEmpty(),size(),clear(),contains(),containsAll(),remove(),removeAll(),equals(),retainAll(),hashCode(),toArray(),iterator()

forEach(){}增强型迭代器，首先从集合中取出一个元素然后赋值给变量，然后输出该变量的值，所以在赋值的时候并不改变集合元素的值

```
Collection coll=new ArrayList();
coll.add(123);
coll.add(234);
coll.add(new String("stine"));
coll.add(false);
/*****迭代器的使用：hasNext(),next(),remove()*****/
//第一种遍历方式---迭代器方式,
Iterator iterator=coll.iterator();
while(iterator.hasNext()){
  System.out.println(iterator.next());
}
//错误写法1
while(iterator.next()!=null){
  System.out.println(itemrator.next());  //会间隔一个元素输出，也会出现NotSuchElementException
}
//错误写法2
while(coll.iterator().hasNext()){
  System.out.println(iterator.next());// 会一直输出第一个元素，每调用一次iterator()方法，迭代器就会重新指回第一个元素的前面一个位置
}
//remove()方法要在next()方法之后使用，否则会报异常

//forEach()
forEach(Object obj:coll){
  obj="as";
}
forEach(Object obj:coll){
  System.out.println(coll);  //此时输出的仍然是原来的集合元素
}

/*****toArray()的使用****/
//将集合转为数组进行迭代
Object[] arr=coll.toArray();
for(int i=0;i<arr.length;i++){
  System.out.println(arr[i]);
}

//将数组转为集合Arrays.asList()
List<String> s=Arrays.asList(new String[]{"aa","ss"});
System.out.println(s);

//注
List list1=Arrays.asList(new int[]{12,23});  //1个元素
List<int[]> list2=Arrays.asList(new int[]{323,12});//1个元素

List list3=Arrays.asList(12,3);//2个元素
List list4=Arrays.asList(new Integer[]{12,34});//2个元素

```

# ArrayList LinkedList Vector的比较       相当于动态数组，会自动扩充容量
1. 三个类都实现了List接口，存储数据的特点相同（存储有序的，可以重复的数据）
2. ArrayList作为List接口的主要实现类  1.2出现    线程不安全，效率高  底层使用Object[]存储
3. Vector作为List的古老实现 1.0出现  比List接口还要早List(1.2)  线程安全，执行效率低
4. LinkedList  1.2出现

## ArrayList源码分析  ArrayList在jdk7和jdk8中稍有不同
ArrayList在jdk7中的情况：
ArrayList list=new ArrayList();   //对于ArrayList空参构造器，默认是在底层创建一个大小为10的Object[] elementData数组
添加元素的时候，首先会判断底层数组容量是否足够，如果足够，就添加元素，否则会进行扩容
扩容机制：
```
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
}
```
第一次扩容为原来容量的1.5倍，如果扩容以后容量还是比实际需要的容量小，则直接将容量设置为实际需要的容量大小；
最终容量如果超过我们设置的阈值private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8; 则直接扩容为Integer.MAX_VALUE
扩容完以后，还需要将原来的数据复制过来
**建议：**在开发中尽量避免扩容操作，这样效率就会更高一些，所以建议使用带参的构造器，ArrayList list=new ArrayList(int capacity);

jdk8中ArrayList的变化： jdk7（饿汉式） jdk8(懒汉式)
ArrayList list=new ArrayList();  //底层Object[] elementData初始化为{}，并没有创建长度为10的数组，延迟了对象的初始化，在第一次调用add()的时候创建数组
后续的添加和扩容机制与jdk7相同

## LinkedList源码解析
LinkedList list=new LinkedList();
LinkedList有两个指针，分别指向第一个节点和最后一个节点；执行add()以后，会调用linkLast(),就是通过last指针将元素添加到链表最后

## Vector源码解析
Vector list13=new Vector();  调用无参构造器，默认初始化为一个大小为10的数组
扩容机制：
```
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                         capacityIncrement : oldCapacity);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```
初始时候，容量默认扩容为原来的2倍，其他的和ArrayList相同，如果扩容还是不够，就直接设为为需要的容量，如果超过自己设置的阈值，则扩容到Integer.MAX_VALUE

# List中的常用方法，相比于Collection，增加了一些关于索引的方法，List作为一种顺序存储结构
（1）add(Integer,Object); //在指定位置插入一个元素Object           addAll(Integer,Collection eles):将集合eles添加到指定位置
ArrayList coll=new ArrayList();
coll.add(12);
coll.add(23);
coll.add(false);
coll.add(new String("stine"));
list.add(1,"12");
（2）get(Integer index);//按照索引获取元素
list.get(1);
(3)获得元素在集合中首次出现的位置indexOf(12)
int index=list.indexOf(23);
System.out.println(index);//如果有元素23，则返回元素首次出现的索引，如果没有，则返回-1
（4）lastIndexOf(Object)  //获得元素组后出现的索引，如果没有则返回-1
System.out.println(lastIndexOf(23))
(5)remove(int index):移除指定位置index处的元素，**并返回此元素**  
这个方法在Collection中也有，但是是按照值删除的，所以这里是重载
Object obj=list.remove(2);
System.out.println(obj);
System.out.println(list);
(6)set(int,value);将指定位置的值改为Value
(7)List sublist(int fromIndex,int toIndex):返回（fromIndex位置的子集合） 包括fromindex不包括toIndex，[)
List sublist=list.subList(2,4);
System.out.println(sublist);

总结：常用方法
（1）增:add(object obj)
（2）删:remove(),remove(int)
（3）改:set(i,value)
（4）查:get(i)
（5）插入:add(int,value)
（6）长度:size()
（7）遍历：iterator;forEach;普通循环size()+get(i)
**remove(int)直接按照索引删除，调用的是collection的方法，如果要按照值进行删除remove(new Integer(2));    所以需要确定自己是要删除索引还是要删除对应值**


## set接口
（1）hashset作为set接口的主要实现类，线程不安全，可以存储null
（2）linkedHashSet：在hashset的基础上增加了一些指针，让其看起来有序，使得在遍历其内部数据时，可以按照添加的顺序遍历，LinkedHashSet的父接口是hashSet
（3）TreeSet：底层通过红黑树实现，要求添加的元素是同一个类型，可以按照对象的某一些属性进行排序   接口Comparable Comparator对象排序接口

**Set接口中没有额外定义新的方法，使用的都是collection中声明过的方法
```
/**
     * Set:
     * (1)无序性：不等于随机性，每次便利的时候都是确定的结果
     *HashSet的遍历输出结果和添加的顺序没有关系，但是每次输出的结果都是一样的
     * LinkedHashSet的遍历输出结果与添加的顺序是一致的
     *原因是：hashset底层也是通过数组实现的，只不过在存储的时候是根据hashCode来决定存储的位置，与实际添加的顺序无关，所以表现出无序性
     *
     * （2）不可重复性
     * 但是当添加两个new对象是可以的，引用的不可重复性
     * set.add(new User("tom",12);
     * set.add(new User("tom",12);//如果User类中没有重写equals(),则会认为这两个对象是不同的对象，通过引用进行比较，则可以同时存在于set中
     * 如果User类中重写equals()，【并且需要写hashCode()】则按照值进行比较，set判断这两个对象是同一个对象，为了满足不可重复性，则只保存一个对象
     * 并且需要写hashCode()，否则还是存储的2个对象User
     *如果没有写hashCode()，则会调用Object中的hashCode()，该hashCode()是通过随机计算的hashCode，同一对象随机获得hashCode
     * 二、添加元素的过程
     * 以hashset为例，实际上是以散列的方式存储的，相同hash值对应的散列值相同，即对应的存储位置相同，不同hash值的对应的散列值也可能相同
     * 添加过程：
     *      当向hashset中添加元素a的时候，首先调用元素a所在类的hashCode()方法，计算元素a的hash值
     *      此hash值接着通过某种算法计算出（散列函数）在底层数组中的存放位置（即索引位置），判断数组此位置上是否已经有元素：
     *          如果此位置上没有其他元素，则元素a添加成功，---------情况1
     *          如果此位置上有其他元素b（或以链表形式存在的多个元素），则比较元素a与元素b的hash值
     *              如果hash值不相同，则添加成功    -----------------情况2
     *              如果hash值相同，则调用元素a的equals方法判断元素a与元素b是否相等，
     *                  equals()返回true，元素a添加失败
     *                  equals()返回false,元素a添加成功   --------------情况3
     * 对于添加成功的情况2和情况3：元素a与已经存在指定索引位置上的数据以链表的方式存储
     * jdk7:元素a放到数组中，指向原来的元素
     * jdk8:原来的元素在数组中，指向元素a
     * 总结：7上8下
     * public boolean equals(Object o){
     *      if(this==o){return true;}
     *     if(o==null||getClass()!=o.getClass()){return false;}
     *     User user=(User) o;
     *     if(age!=user.age)return false;
     *     return name!=null?name.equals(user.name):user.name==null;
     * }
     * public int hashCode(){  //return name.hashCode()+1
     *     int result=name!=null?name.hashCode():0;
     *     result=31*result+age;   //31可以减少冲突，系数越大，计算出的hash地址越大，所谓的冲突就越小，查找起来效率也会提高
     *     return result;//但是也不能太大，会造成溢出31只占5bit，相乘造成数据溢出的概率较小
     * }//31可以由i*31==(i<<5)-1来表示，现在许多虚拟机都有做相关优化，提高算法效率
     * //31是一个素数，素数作用就是如果用一个数字乘以素数，那么最终出来的结果只能被素数本身和被素数和1整除（减少冲突）
     *
     * 要求：向set中添加的类对象数据，其所在的类一定要重写hashCode()和equals()
     * 重写的hashCode()和equals()尽可能保持一致性：相等的对象必须具有相等的散列码
     * new一个hashSet，其实底层就是一个hashMap
     *
     * LinkedHashSet:
     * LinkedHashSet作为HashSet的子类，再添加数据的同时，每一个数据还维护了两个引用，记录此数据前一个和后一个数据
     * 优点：对于频繁的遍历操作，LinkedHashSet效率高于HashSet
     *
     * TreeSet：
     *向TreeSet中添加的数据，要求是相同类的对象
     *遍历的结果按照从小到大的顺序输出      字符串按照对应的ascll码排序，字典序
     * 如果网TreeSet中添加的是类对象User{name,age},就可以由2中方式进行排序遍历输出
     * 两种排序方式：自然排序（Comparable）和定制排序（Comparator）
     * 直接使类实现Comparable接口 java.lang
     *自然排序中，比较两个对象是否相同的标准为：compareTo()返回0，不再是equals()
     *
     * TreeSet和TreeMap采用红黑树的存储结构，有序，查询速度比List快
     */
```
User.java
```
package com.atguigu.gulimall.search.thread;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@AllArgsConstructor
@NoArgsConstructor
@Data
public class User implements Comparable{
    private String name;
    private int age;

    @Override
    public int compareTo(Object o) {  //按照姓名从小到大排列

        if(o instanceof User){
            User user=(User)o;
//            return this.name.compareTo(user.name);
//            return -this.name.compareTo(user.name);//从大到小
            int compare= -this.name.compareTo(user.name);
            if(compare!=0){
                //直接根据名字判断返回
                return compare;
            }else{
                return Integer.compare(this.age,user.age);//默认年龄从小到大排列
            }
        }else{
            throw new RuntimeException("输入的类型不匹配");
        }

    }
}
```
public static void main(String[] args) {

        Vector v=new Vector();

        //TreeSet定制排序
        Comparator com=new Comparator() {
            //按照年龄从小到大排列
            @Override
            public int compare(Object o1, Object o2) {
                if (o1 instanceof User && o2 instanceof User) {
                    User u1=(User)o1;
                    User u2=(User)o2;
                    return Integer.compare(u1.getAge(),u2.getAge());
                }else{
                    throw new RuntimeException("输入的数据类型不匹配");
                }
            }
        };
        TreeSet set1=new TreeSet(com);
        set1.add(new User("Tom",12));
        set1.add(new User("Jerry",32));
        set1.add(new User("Jim",2));
        set1.add(new User("Mike",65));
        set1.add(new User("Jack",33));
        set1.add(new User("Jack",56));

        Iterator iterator1 = set1.iterator();
        while(iterator1.hasNext()){
            System.out.println(iterator1.next());
        }

        System.out.println("===============================================");

        TreeSet set=new TreeSet();
        //失败：不能添加不同类的对象
//        set.add(123);
//        set.add(456);
//        set.add("AA");
//        set.add(new String("tom"));
//          set.add(34);
//          set.add(323);
//          set.add(-12);
//          set.add(2);
        //String类型
//        set.add("as");
//        set.add("Stine");
//        set.add("bf");

        set.add(new User("Tom",12));
        set.add(new User("Jerry",32));
        set.add(new User("Jim",2));
        set.add(new User("Mike",65));
        set.add(new User("Jack",33));
        set.add(new User("Jack",56));   //如果User中接口的实现只实现了通过名字进行比较，则这两个会被认为是同一个元素，而丢弃第二个元素
        //如果接口中实现了对年龄比较，则属于不同元素
        Iterator iterator = set.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }

////        Set set=new HashSet();
//        Set set=new LinkedHashSet();
//        set.add(456);
//        set.add(23);
//        set.add("aa");
//        set.add(43);
//        set.add("12");
//        Iterator iterator = set.iterator();
//        while(iterator.hasNext()){
//            System.out.println(iterator.next());
//        }


//        ArrayList coll=new ArrayList();
//        coll.add(123);
//        coll.add(456);
//        coll.add(new String("jerry"));
//        coll.add(false);
//        System.out.println(coll);
//        coll.add(1,"bb");
//        System.out.println(coll);
//        System.out.println(coll.hashCode());

//        Iterator iterator = coll.iterator();
//        for (int i = 0; i < coll.size(); i++) {
//            System.out.println(iterator.next());
//        }

//        while(iterator.hasNext()){
//            System.out.println(iterator.next());
//        }
//
//        //forEach遍历集合
//        for (Object o : coll) {
//            System.out.println(o);
//        }
        /**
         * foreach首先将集合元素取出来，然后复制给变量
         */
//        for (Object o : coll) {
//            o="GG";   //这里修改的是取出来的元素赋值的元素o
//            //所以原来的集合中的元素没有改变
//        }
//        for (Object o : coll) {
//            System.out.println(o);
//        }


//        //集合转为数组,以数组的方式遍历
//        Object[] arr=coll.toArray();
//        for(int i=0;i<arr.length;i++){
//            System.out.println(arr[i]);
//        }
//
//        //将数组转为集合
//        List<String> list = Arrays.asList(new String[]{"aa", "bb", "cc"});
//        System.out.println(list);
//
//        List arr1=Arrays.asList(new int[]{123,232});
//        System.out.println(arr1.size());    //1
//
//        List<int[]> arr2=Arrays.asList(new int[]{3434,23});
//        System.out.println(arr2);//1
//
//        //获得2个元素
//        List list1=Arrays.asList(123,132);
//        System.out.println(list1);  //2
//
//        List list2=Arrays.asList(new Integer[]{23,43});
//        System.out.println(list2);//2

        //iterator():返回iterator接口的实例，用于遍历集合元素，


//        ArrayList list11=new ArrayList();
//        LinkedList list12=new LinkedList();
//        Vector list13=new Vector();
}
```
```
## Map
```
/**
     * Map接口
     *                          map
     *         -------------------------------------------------
     *         |                   |                    |
     *         Hashtable(实现类)   HashMap              SortedMap
     *         |                   |                     |
     *         Properties        LinkedHashMap         TreeMap（实现类）
     *
     * Map：双列数据，存储key-value数据  类似于函数概念
     *   HashMap：作为Map的主要实现类   jdk:1.2   线程不安全  ，效率比较高  可以存储null的key和value
     *  Hashtable:作为古老的实现类  jdk1.0      线程安全，效率比较低        不可以存储null的key和value
     *  TreeMap:jdk1.2   linkedHashMap:jdk1.4   map接口：jdk1.2
     *
     *LinkedHashMap：保证在遍历map元素时，可以按照添加的顺序实现遍历
     * 原因：在原有的HashMap底层结构基础上，添加一对指针，只想前驱节点和后继节点 ，对于频繁的遍历操作，此类执行效率该与HashMap
     *
     * TreeMap:保证按照添加的key-value对进行排序，实现排序遍历，通过key进行排序（自然排序或者定制排序）
     *          底层使用红黑树
     *
     * HashMap底层：数组+链表（jdk7及以前）
     *              数组+链表+红黑树（jdk8）
     *
     * Properties:常用来处理配置文件，key-value都是String类型
     *
     * 面试题：
     * （1）HashMap的底层实现原理
     * （2）HashMap与Hashtable之间的异同
     * （3）CurrentHashMap与Hashtable之间的异同？
     *
     * 二、Map结构的理解
     * Map中的key：无序的、不可重复的，使用Set存储所有的key     --------->key所在的类要重写equals()和hashCode()
     * Map中的value：无序的、可重复的，使用Collection存储所有的value---->value所在的类要重写equals()
     * 一个键值对：key-value构成一个entry对象
     * map中的entry：无序的，不可重复的，使用Set存储所有的entry
     *
     * ** 三、HashMap的底层实现**
     *以空参构造器为例：HashMap map=new HashMap();
     * 在实例化以后，底层创建了一个长度是16的一维数组Entry[] table.
     *.........可能已经执行过多次put..........
     * map.put(key1,value1)
     * 首先调用key1所在类的hashCode()计算得到一个hashCode，然后通过散列函数进行散列到entry[] table的某一个索引
     * 如果此位置上为空，则此时的<key1,value1>添加成功       ----------------》情况1
     * 如果此位置上不为空（意味着此位置上存在一个或多个数据（以链表形式存在）），比较key1和已经存在的一个或多个数据
     * 的哈希值：
     *          如果key1的哈希值与已经存在的数据的哈希值都不相同，此时key1-value1添加成功  -----------------》情况2
     *          如果key1的哈希值与已经存在的某一个数据<key2,value2>的哈希值相同，继续比较，调用key1所在类的equals(key2)
     *                  如果equals()返回false:此时key1-value1添加成功    -------------------》情况3
     *                  如果equals()返回true：使用value1替换value2
     *
     *    补充：关于情况2和情况3：此时key1-value1和原来的数据以链表的方式存储
     *    在不断的添加过程中，会涉及到扩容问题，默认的扩容方式，扩容为原来容量的2倍，并将原有的数据复制过来
     *
     *    jdk8相较于jdk7在底层实现方面的不同：
     *    (1)new HashMap():底层没有创建一个长度为16的数组       类似于ArrayList在jdk7和jdk8之间的区别
     *    （2）jdk8底层的数组是：Node[],而非Entry[]
     *    (3)在第一次put()方法时，底层创建长度为16的数组
     *    （4）jdk7底层结构只有：数组+链表  jdk8中底层结构：数组+链表+红黑树
     *    当数组的某一个索引位置上的元素以链表形式存在的数据个数>8 且当前数组的长度》64时，
     *    此时此索引位置上的所有数据改为使用红黑树存储
     */
```
### 源码分析
```
DEFAULT_LOAD_FACTOR:HashMap的默认加载因子：0.75
     *    threshold：扩容的临界值=容量*填充因子
     * static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
     * TREEIFY_THRESHOLD：Bucket中链表长度大于该默认值，转化为红黑树：8
     * MIN_TREEIFY_CAPACITY   ：桶中的Node被树化时最小的hash表容量：64
     *
     *三、LinkedHashMap的底层实现原理
     *  LinkedHashMap:底层也是调用其父类HashMap的put方法，只不过重写了newNode方法
     * 在HashMap中的Node定义为：
     *          final int hash;
     *         final K key;
     *         V value;
     *         Node<K,V> next;
     *         
     * LinkHashMap中的Entry继承了HaspMap中的Node：
     * static class Entry<K,V> extends HashMap.Node<K,V> {
     *         Entry<K,V> before, after;
     *         Entry(int hash, K key, V value, Node<K,V> next) {
     *             super(hash, key, value, next);
     *         }
     *     } //相比较于父类HashMap.Node多定义了指向前后节点的before和after
     *所以LinkedHashMap可以按照添加元素的顺序实现遍历
     *【注】在HashSet中，实际上就是将值存放到HashMap的key的位置，对应的Value位置存放的是PRESENT=new Object();
     * 所以HashSet底层实现基于HashMap   所以HashSet添加元素的过程与HashMap类似
     *
     *
     * 四、Map中常用的方法   以HashMap为例  4+6+3=13个常用方法
     *(1)基本操作方法：
     * put() ,putAll(),remove(),clear()
     * (2)size(),isEmpty(),equals(),get(),containsKey(),containsValue()
     *(3)遍历方法（元视图操作方法）：    map没有iterator
     * Set keySet():返回所有key构成的Set集合
     * Collection Values():返回所有value构成的Collection集合
     * Set entrySet():返回所有key-value对构成的Set集合
     *
     *五、TreeMap
     * 向TreeMap中添加key-value,要求key必须是由同一个类创建的对象
     *因为要按照key进行排序：自然排序，定制排序（只能按照key进行排序，不能按照value排序）
     *
     * 六、Proerties
```
