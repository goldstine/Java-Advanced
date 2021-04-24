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







