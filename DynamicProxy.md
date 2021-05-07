# 动态代理
反射的应用，体现了反射的动态性
## 静态代理
```
package com.atguigu.gulimall.search.thread.proxy;

import org.junit.Test;

/**
 * 静态代理:代理类和被代理类在编译期间就确定下来了
 */
interface ClothFactory{
    void produceCloth();
}
//代理类
class ProxyClothFactory implements ClothFactory{

    private ClothFactory factory;//用被代理类对象进行实例化
    public ProxyClothFactory(ClothFactory factory){
        this.factory=factory;
    }
    @Override
    public void produceCloth() {
        System.out.println("代理工厂做一些准备工作");
        factory.produceCloth();
        System.out.println("代理工厂做一些后续的收尾工作");
    }
}
//被代理类
class NikeClothFactory implements ClothFactory{

    @Override
    public void produceCloth() {
        System.out.println("耐克工厂生产一批运动服");
    }
}
public class StaticProxyTest {

    @Test
    public void test1(){
        NikeClothFactory nikeClothFactory = new NikeClothFactory();
        //创建代理类对象
        ProxyClothFactory proxyClothFactory = new ProxyClothFactory(nikeClothFactory);
        proxyClothFactory.produceCloth();

    }
}
```
## 动态代理
```
package com.atguigu.gulimall.search.thread.proxy;

import org.junit.Test;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
 * 动态代理
 */

interface Human{
    String getBelief();
    void eat(String food);
}
//被代理类
class SuperMan implements Human{

    @Override
    public String getBelief() {
        return "i believe i can fly!";
    }

    @Override
    public void eat(String food) {
        System.out.println("我喜欢吃"+food);
    }
}

/**
 * 要想实现动态代理，需要解决的问题
 * 问题一：如何根据加载到内存的被代理类，动态的创建一个代理类及其对象
 * 问题二：当通过代理类的对象调用方法时，如何动态地去调用被代理类中的同名方法
 *
 */
class ProxyFactory{
    //通过被代理类obj返回代理类对象
    public static Object getProxyInstance(Object obj){
        MyInvocationHandler myInvocationHandler = new MyInvocationHandler();
        myInvocationHandler.bind(obj);
       return  Proxy.newProxyInstance(obj.getClass().getClassLoader(),obj.getClass().getInterfaces(),myInvocationHandler);
    }
}
class MyInvocationHandler implements InvocationHandler{
    //当我们通过代理类的对象，调用方法时，就会自动地调用如下方法:invoke()
    //将被代理类要执行的方法的功能声明在invoke()中
    private Object obj;//赋值时，也需要使用被代理类对象

    public void bind(Object obj){
        this.obj=obj;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        HumanUtils humanUtils = new HumanUtils();
        humanUtils.method1();

        //proxy表示代理类的对象，method是代理类对象调的方法
        Object invoke = method.invoke(obj, args);

        humanUtils.method2();

        return invoke;//代理类对象或被代理类对象方法的返回值

    }
}

class HumanUtils{
    public void method1(){
        System.out.println("=======================通用方法一========================");

    }
    public void method2(){
        System.out.println("==============================通用方法二==================");
    }
}

public class ProxyTest {
    
    @Test
    public void test1(){
        SuperMan superMan = new SuperMan();
        Human proxyInstance =(Human) ProxyFactory.getProxyInstance(superMan);
        //当通过代理类对象调用方法时，会自动的调用被代理类中同名的方法
        String belief = proxyInstance.getBelief();
        System.out.println(belief);
        proxyInstance.eat("四川麻辣烫");
        System.out.println("+++++++++++++++++++++++++++++++++++++++");

        NikeClothFactory nikeClothFactory = new NikeClothFactory();
        ClothFactory proxyInstance1 = (ClothFactory) ProxyFactory.getProxyInstance(nikeClothFactory);
        proxyInstance1.produceCloth();
    }

}
```
