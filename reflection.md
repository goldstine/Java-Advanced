+ 疑问1：通过直接new的方式或反射的方式都可以调用公共的结构，开发中到底用哪一个？
+ 建议：直接new的方式
+ 什么时候会使用：反射的方式，反射的特征：动态性，前后端分离项目中使用反射比较多，后端在运行中需要动态的创建对象调用属性和方法
+ 疑问2：反射机制与面向对象中的封装性是不是矛盾？如何看待两个技术？
+ 不矛盾

（1）动态语言</br>
是一类在运行时可以改变其结构的语言：例如新的函数、对象，甚至代码可以被引进，已有的函数可以被删除或是其他结构上的变化：通俗说就是在运行时代码可以根据某些条件改变自身结构</br>
（2）静态语言</br>
与动态语言相对应的，运行时结构不可变的语言就是静态语言，如java，c,c++

java不是动态语言，但java可以称之为准动态语言，即java有一定的动态性，可以利用反射机制，字节码操作获得类似于动态语言的特性




```
 /**
     * 关于java.lang.Class类的理解
     *（1）类的加载过程
     * 一个java程序首先通过javac.exe命令生成一个或多个字节码文件（xxx.class）（编译）,然后通过java.exe对某一个字节码文件进行解释运行，相当于将该字节码文件加载进内存
     * 此过程就称为类的加载，此加载到内存的字节码文件，称为运行时类，称为Class的一个实例
     *换句话说，Class的实例对应这一个运行时类
     *
     * 如何获取Class实例（4种方法）
     *加载到内存中的运行时类，会缓存一段时间，在此时间内，我们可以通过不同的方式来获取此运行时类
     *全部都指向这一个运行时类，完全没有必要创建多个运行时类对象
     *第一种方式写死了，直接编译时就会判断是否写错了        
     * 第二种方式，首先需要拿到对象，有了对象就没有必要拿类了
     * 第三种方式：编译时不会报错，运行时会报错，所以第三种方式用的比较多
     * 第四种方式了解
     *
     *注解、枚举类、数组、基本数据类型，接口都算是Class的实例
     *哪些可以作为Class的实例？
     * （1）class：外部类，成员（成员内部类、静态内部类），局部内部类，匿名内部类
     * （2）interface接口
     * （3）[]，数组
     * （4）enum：枚举
     * （5）annotation:注解@interface
     * （6）primitive type:基本数据类型
     * （7）void
     *
     *创建运行时类的对象，通过反射的方式创建加载进内存的运行时类的对象
     *
     *获取运行时类的完整结构
     *
     */
    
    
    @Test
    public void test() throws ClassNotFoundException {
        //四种方式获取的都是同一个运行时类，所以clazz==clazz3==clazz2

        //方式一:调用运行时类的属性，class
        Class<Person> clazz = Person.class;
        System.out.println(clazz);
        //方式二：任何一个对象都知道是由哪一个类生成的，在Object中定义的getClass()
        Person p1=new Person();
        Class clazz2 = p1.getClass();
        System.out.println(clazz2);

        System.out.println(clazz==clazz2);

        //方式三：调用Class的静态方法：forName(String classPath)

        Class clazz3=Class.forName("com.atguigu.gulimall.search.thread.Person");//和操作文件类似，会存在找不到该Person的异常
        System.out.println(clazz3);
        System.out.println(clazz==clazz3);

        Class clazz4 = Class.forName("java.lang.String");
        System.out.println(clazz4);


        //方式四：使用类的加载器：ClassLoader
        ClassLoader classLoader = ReflectionTest.class.getClassLoader();
        Class clazz5 = classLoader.loadClass("com.atguigu.gulimall.search.thread.Person");
        System.out.println(clazz5);
        System.out.println(clazz==clazz5);
    }
    
        //可以作为Class实例
    @Test
    public void test2(){
        Class c1=Object.class;
        Class c2=Comparable.class;
        Class c3=String[].class;
        Class c4=int[][].class;
        Class c5= ElementType.class;
        Class c6=Override.class;
        Class c7=int .class;
        Class c8=void.class;
        Class c9=Class.class;  //Class本身作为Class实例

        int[] a=new int[10];
        int[] b=new int[100];
        Class c10=a.getClass();
        Class c11=b.getClass();
        //只要数组元素类型与维度一样，就是同一个Class   都是以为整型数组
        System.out.println(c10==c11);
    }

    @Test
    public void test3(){
        //对于自定义类，使用系统类加载器进行加载
        ClassLoader classLoader=ReflectionTest.class.getClassLoader();
        System.out.println(classLoader);//获得当前类的（自定义类）类加载器
        //通过系统类加载器的getParent()可以获得扩展类加载器
        ClassLoader classLoader1=classLoader.getParent();
        System.out.println(classLoader1); //系统类加载器的父加载器是扩展加载器
        //通过扩展类的getParent()，无法获得引导类加载器
        //引导类加载器主要负责加载java核心类库，无法加载自定义类的
        ClassLoader parent = classLoader1.getParent();
        System.out.println(parent);
        //核心类库的加载
        ClassLoader classLoader2 = String.class.getClassLoader();
        System.out.println(classLoader2);//String也不能获取到类加载器，说明String也是由引导类加载器加载的
    }

    /**
     * Properties：用来读取配置文件
     */
    @Test
    public void test4() throws IOException {
        Properties pros=new Properties();
        //此时的文件默认在当前的module下
        //读取配置文件的方式一
//        FileInputStream fis=new FileInputStream("D:\\Goldstine_workspace\\JavaDevelopment\\Multi_Thread_project\\gulimall\\gulimall-search\\src\\main\\resources\\jdbc.properties");
        FileInputStream fis=new FileInputStream("src\\main\\resources\\jdbc.properties");//这样写也是可以的
        pros.load(fis);   //配置文件默认识别为module的src下

        //方式二：通过类加载器加载配置文件
//        ClassLoader classLoader=ReflectionTest.class.getClassLoader();
//        InputStream is=classLoader.getResourceAsStream("jdbc.properties");// 配置文件放在resource文件夹下
//
//        pros.load(is);

        String user=pros.getProperty("user");
        String password=pros.getProperty("password");
        System.out.println("user="+user+",password="+password);

    }

//    创建运行时类的对象，通过反射的方式创建加载进内存的运行时类的对象
    @Test
    public void test5() throws ClassNotFoundException, IllegalAccessException, InstantiationException {
        Class clazz=Class.forName("com.atguigu.gulimall.search.thread.Person");
        Object obj = clazz.newInstance();   //如果创建Class实例的时候，如果指定了泛型，那么newInstance()就会自动识别为Person对象
        //newInstance()实际上还是调用的Person的空参构造器，所以要想使用newInstance()创建一个对象，就必须在该对象的类中给出一个空参构造器，否则会抛出对应的异常  InstantiationException针对空参构造器抛的异常
        //并且该空参构造器的访问属性不能为private，可以为public或默认的包访问属性  IllegalAccessException:针对空参构造方法权限问题抛的异常
        //类似于子类调用父类的super()，父类提供一个空参构造器， 通常在javabean中需要提供一个public的空参构造器
            //原因：（1）便于通过反射，创建运行时类对象，（2）便于子类继承此运行时类时，默认调用super()时，保证父类有此构造器
        //只要是造对象，都是通过构造器造的对象，只不过表现的形式不同而已，有的是通过new一个对象，有的是通过运行时类newInstance()一个对象

        System.out.println(obj);
        Person person=(Person)obj;


    }

    //反射的动态性
    @Test
    public void test6() throws Exception {

        for(int i=0;i<100;i++) {
            int num = new Random().nextInt(3);//0,1,2   只有在运行时才能确定创建哪一个运行时类的对象
            String classPath = "";
            switch (num) {
                case 0:
                    classPath = "java.util.Date";
                    break;
                case 1:
                    classPath = "java.lang.Object";
                    break;
                case 2:
                    classPath = "com.atguigu.gulimall.search.thread.Person";
                    break;
            }
            System.out.println(getInstance(classPath));
        }
    }

    //创建运行时类对象
    public Object getInstance(String classPath) throws Exception {
        Class clazz = Class.forName(classPath);
        return clazz.newInstance();
    }

    
```
