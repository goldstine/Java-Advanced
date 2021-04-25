### IO流
流的分类：
+ 按操作的数据单位不同可以分为：字节流和字符流
+ 按数据流的流向不同分为：输入流与输出流
+ 按流的角色不同分为：节点流，处理流

|抽象基类|字节流|字符流|
|-------|------|-----|
|输入流|  InputStream|Reader|
|输出流| OutputStream|Writer|
---------------------------
由这四个类派生出来的子类名称都是以其父类名作为子类名后缀
## IO流体系
|分类|字节输入流|字节输出流|字符输入流|字符输出流|
|----|---------|---------|---------|---------|
|抽象基类|InputStream|OutputStream|Reader|writer|
|访问文件|FileInputStream|FileOutputStream|FileReader|FileWriter|
|访问数组|ByteArrayInputStream|ByteArrayOutputStream|CharArrayReader|CharArrayWriter|
|访问管道|PipedInputStream|PipedOutputStream|PipedReader|PipedWriter|
|访问字符串|||StringReader|StringWriter|
|缓冲流|BufferedInputStream|BufferedOutputStream|BufferedReader|BufferedWriter|
|转换流|||InPutStreamReader|OutputStreamWriter|
|对象流|ObjectInputStream|ObjectOutputStream|||
|打印流||PrintStream||PrintWriter|
|推回输入流|PushbackInputStream||PushBackReader||
|特殊流|DataInputStream|DataOutputStream|||
--------------------------------------------

抽象基类：             InputStream    outputStream          Reader         Writer</br>
节点流（文件流）：FileInputStream     FileOutputStream      FileReader      FileWriter</br>
处理流：      BufferedInputStream   BufferedOutputStream   BufferedReader   BufferedWriter  </br>   作用在上面的节点流之上

**以FileReader为例**
（1）实例化File对象：File file=new File("path");
(2)指定一个流：FileReader fr=new FileReader(file);
(3)读取一个字符，返回读的结果，如果读到文件的结尾，则返回-1
int data=fr.read();
while(data!=-1){
  System.out.println((char)data);
  data=fr.read();
}
(4)关闭流
fr.close()

### 文件的基本操作
```
public class FileReaderWriterTest {

//    public static void main(String[] args) {
//        File file=new File("hello.txt");
//        System.out.println(file.getAbsolutePath());//相较于当前的工程下gulimall：D:\Goldstine_workspace\JavaDevelopment\Multi_Thread_project\gulimall\hello.txt
//          File file=new File("gulimall\\hello.txt");
//    }

    /**
     * 将hello.txt文件内容读入程序中，并输出到控制台中
     */
    @Test
    public void test1() /*throws IOException*/ {
        FileReader fr= null;

        try {
            //1、实例化File对象，指明要操作的文件对象
            File file=new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\hello.txt");//文件地址相较于当前的模块gulimall-search
            System.out.println(file.getAbsolutePath());//D:\Goldstine_workspace\JavaDevelopment\Multi_Thread_project\gulimall\gulimall-search\hello.txt
            //2、提供具体的流
            fr = new FileReader(file);//可能会出现文件不存在异常，读入的文件一定要存在，否则就会报FileNotFoundException

            //3、数据的读入
//        int data = fr.read();  //read()：返回读入的一个字符，如果达到文件末尾，返回-1
//        while (data != -1) {
//            System.out.print((char)data);
//            data=fr.read();//继续读下一个字符
//        }
            //方式二:语法上针对方式一的修改
            int data;
            while((data=fr.read())!=-1){
                System.out.print((char)data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            //4、流的关闭操作
            try {
                if(fr!=null) {
                    fr.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
            //或
            if(fr!=null){
                try {
                    fr.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
            }

        }

        /**
         * 如果采用throws进行异常处理，会出现不能关闭流，的资源泄露问题
         * 所以最好采用try catch的方式处理异常
         */
    }
}
```
#### 读取数据的改进 FileReader
```
@Test
    public void test2(){
        FileReader fr= null;
        try {
            //对read()操作的升级，使用read的重载方法
            File file=new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\hello.txt");
            fr = new FileReader(file);
            char[] cbuf=new char[5];
            int len;
            while((len=fr.read(cbuf))!=-1){
//                for(int i=0;i<len/*cbuf.length*/;i++){// 出现的问题，每次都读5个字符，如果文件中是：helloworld123，则读出来的数据是：helloworld123ld
//                    System.out.print(cbuf[i]);//cbuf.length是错误的写法
//                }

                //方式二：
//                String str=new String(cbuf);//错误写法：输出为helloworld123ld，对应着方式一的错误写法
//                System.out.print(str);
                //正确的写法
                String str=new String(cbuf,0,len);
                System.out.print(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4、资源的关闭
            try {
                if(fr!=null) {
                    fr.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
```
## 将文件数据从内存中写入磁盘中
```
/**
     * 从内存中写出数据到硬盘的文件里
     * 说明：（1）如果该文件不存在，则会在写的时候自动创建该文件，不会抛异常
     * （2）如果该文件存在，则在写的时候默认是将源文件覆盖
     */
    @Test
    public void test3() throws IOException {
        //1、提供File对象,   写出的时候文件output.txt文件不存在，但是运行的时候自动创建该文件，不会报FileNotFoundException
        File file=new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\output.txt");
        //2、提供写出文件的流，提供FileWriter的对象，用于数据的写出
        FileWriter fw=new FileWriter(file,false);//false表示当文件存在的时候，是否在该文件的基础上覆盖还是追加，false表示覆盖，true表示追加

        //3、写文件
        fw.write("I have a dream!\n");
//        fw.write("I have a dream!".toCharArray());
        fw.write("you need to have a dream!");


        //4、关闭流资源
        fw.close();
    }
```
## 将文件从磁盘中读出，然后将该文件写入磁盘 （相当于文件的复制）
```/**
     * 将文件从磁盘读入内存，又从内存中将该文件输出到磁盘中
     * 相当于对该文件进行复制
     */
    @Test
    public void test4() {
        FileReader fr1= null;
        FileWriter fw1= null;
        try {
            //1、创建File对象
            File file1=new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\hello.txt");
            //2、创建读字符流FileReader
            fr1 = new FileReader(file1);

            //5、创建写入的File文件
            File file2=new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\output1.txt");
            //6、创建输出文件的流FileWriter
            fw1 = new FileWriter(file2);

            //3、读文件，采用字符数组的方式
            char[] cbuf=new char[5];
            int len;
            while((len=fr1.read(cbuf))!=-1){
//                String str=new String(cbuf,0,len);
//                fw1.write(str);
                    fw1.write(cbuf,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //方式一：并列着写
//            if(fr1!=null) {
//                //4、关闭流资源
//                try {
//                    fr1.close();
//                } catch (IOException e) {
//                    e.printStackTrace();
//                }
//            }//即使第一个关闭流会出现异常，后面的关闭也会继续执行
//            if(fw1!=null) {
//                try {
//                    fw1.close();
//                } catch (IOException e) {
//                    e.printStackTrace();
//                }
//            }
            //方式二，嵌套着写
            try {
                if(fr1!=null) {
                    fr1.close();
                }
            }catch(IOException e){
                e.printStackTrace();
            }finally {
                try {
                    if(fw1!=null) {
                        fw1.close();
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```
**不能使用字符流来处理图片等字节数据，要实现对图片和视频数据的复制，就需要使用字节流：InputStream ,OutputStream**

## 字节流 InputStream和OutputStream
(1)使用字节流读取文本文件，可能会出现乱码
（2）字节流实现对图片的复制
（3）指定路径下文件的复制（字节流可以实现文本文件的复制，但是不能用来读取文本文件）
```
@Test
    public void test1() {
        /**
         * 说明：如果是纯英文文本，其实一个英文字母和数字还是使用的一个字节Byte存储的，所以使用字节流去读写，也是可以的
         * 但是对于中文汉字的文本，一个汉字，需要3个Byte存储，此时如果通过字节流进行读写就会乱码
         *结论：
         * 1、对于文本文件（.txt,.java,.c.cpp），使用字符流进行处理
         * 2、对于非文本文件(.jpg,.mp3,.mp4,.avi,.doc,.ppt)，使用字节流处理
         */
        FileInputStream fis= null;
        try {
            //1、创建文件
            File file=new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\hello.txt");
            //2、创建字节流
            fis = new FileInputStream(file);

            //3、读数据
            byte[] buffer=new byte[5];
            int len;//记录每次读取的字节的个数
            while((len=fis.read(buffer))!=-1){
                String str=new String(buffer,0,len);
                System.out.print(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4、关闭流资源
            try {
                if(fis!=null) {
                    fis.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 实现对图片的一个复制
     */
    @Test
    public void test2() {
        FileInputStream fis= null;
        FileOutputStream fos= null;
        try {
            File srcFile=new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\0.png");
            File destFile=new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\1.png");

            fis = new FileInputStream(srcFile);
            fos = new FileOutputStream(destFile);

            byte[] buffer=new byte[5];
            int len;
            while((len=fis.read(buffer))!=-1){
                fos.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if(fis!=null) {
                    fis.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            } finally {

                try {
                    if(fos!=null) {
                        fos.close();
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

        }
    }

    /**
     * 指定路径下文件的复制
     * 文本文件使用字符流，非文本文件使用字节流
     * 【说明】如果只是要复制文本文件，使用字节流FileInputStream和FileOutputStream也是可以的，但如果是要读，就不能使用字节流读文本文件，会乱码
     *但如果是字节文件（非文本文件）不能使用字符流进行复制
     */
    @Test
    public void test3(){
        //记录复制的时间
        long start=System.currentTimeMillis();
        String srcPath="src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\hello.txt";
        String destPath="src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\hello1.txt";
        copyFile(srcPath,destPath);


        long end=System.currentTimeMillis();
        System.out.println("复制操作花费的时间为："+(end-start));
    }

    public void copyFile(String srcPath,String destPath){
        FileInputStream fis= null;
        FileOutputStream fos= null;
        try {
            File srcFile=new File(srcPath);
            File destFile=new File(destPath);

            fis = new FileInputStream(srcFile);
            fos = new FileOutputStream(destFile);

            byte[] buffer=new byte[1024];
            int len;
            while((len=fis.read(buffer))!=-1){
                fos.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if(fis!=null) {
                    fis.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            } finally {

                try {
                    if(fos!=null) {
                        fos.close();
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

        }
    }
```

