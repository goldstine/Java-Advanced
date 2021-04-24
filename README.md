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
