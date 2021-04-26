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
## 缓冲流 BufferedInputStream BufferedOutputStream (缓冲字节流)| BufferedReader BufferedWriter(缓冲字符流)
**应用：实现图片的加密和解密；对文本文件字符出现的个数进行Map统计**
```
/**
     * 缓冲流的使用
     * （1）BufferedInputStream
     * (2)BufferedOutputStream
     * (3)BufferedReader
     * (4)BufferedWriter
     *
     * 缓冲流的作用：提高流的读取和写入的速度
     */

    /**
     * 实现非文本文件的复制
     */
    @Test
    public void test1() {
        BufferedInputStream bis= null;
        BufferedOutputStream bos= null;
        try {
            //1、造文件
            File srcFile=new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\0.png");
            File destFile=new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\2.png");
            //2、创建流
            //2.1创建节点流
            FileInputStream fis=new FileInputStream(srcFile);
            FileOutputStream fos=new FileOutputStream(destFile);
            //2.2创建缓冲流
            bis = new BufferedInputStream(fis);
            bos = new BufferedOutputStream(fos);
            //3、复制的细节，读取和写入
            byte[] buffer=new byte[10];
            int len;
            while((len=bis.read(buffer))!=-1){
                bos.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {

            try {
                if(bos!=null) {
                    bos.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

            try {
                if(bis!=null) {
                    bis.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        //4、资源关闭,要求：先关闭外层的流，再关闭内层的流

        //说明：关闭外层流的同时，内层流也会自动进行关闭；关于内层流的关闭，我们可以省略
//        fos.close();
//        fis.close();

    }

    /**
     * 实现文件复制的方法，对比BufferedInputStream和BufferedOutputStream比InputStream和OutputStream更快
     * 这里可以采用复制视频的方式比较，一般字符缓冲流的速度更快
     * 缓冲流能够提高读写速度的原因是因为底层通过创建一个缓冲区8*1024
     */
    @Test
    public void test2(){
        long start=System.currentTimeMillis();

        String srcPath="src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\0.png";

        String destPath="src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\3.png";

        copyFile(srcPath,destPath);

        long end=System.currentTimeMillis();
        System.out.println("赋值操作花费的时间为："+(end-start));
    }

    public void copyFile(String srcPath,String destPath){
        BufferedInputStream bis= null;
        BufferedOutputStream bos= null;
        try {
            //1、造文件
            File srcFile=new File(srcPath);
            File destFile=new File(destPath);
            //2、创建流
            //2.1创建节点流
            FileInputStream fis=new FileInputStream(srcFile);
            FileOutputStream fos=new FileOutputStream(destFile);
            //2.2创建缓冲流
            bis = new BufferedInputStream(fis);
            bos = new BufferedOutputStream(fos);
            //3、复制的细节，读取和写入
            byte[] buffer=new byte[1024];
            int len;
            while((len=bis.read(buffer))!=-1){
                bos.write(buffer,0,len);

//                bos.flush();//可以强制将缓冲区中的内容写出或者读入，其实底层已经自动flush()，达到一定的阈值就会自动flush()

            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {

            try {
                if(bos!=null) {
                    bos.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

            try {
                if(bis!=null) {
                    bis.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 测试缓冲字符流
     */
    @Test
    public void test3()  {
        BufferedReader br= null;
        BufferedWriter bw= null;
        try {
            //创建文件
            File srcFile=new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\hello.txt");
            File destFile=new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\output2.txt");
            //创建流
            FileReader fr=new FileReader(srcFile);
            FileWriter fw=new FileWriter(destFile);

            br = new BufferedReader(fr);
            bw = new BufferedWriter(fw);

            //读写文件   方式一
//            char[] buffer=new char[10];
//            int len;
//            while((len=br.read(buffer))!=-1){
//                bw.write(buffer,0,len);
//            }
            //方式二
            String data;
            while((data=br.readLine())!=null){
                //方式一：直接添加换行\n
//                bw.write(data+"\n");//data中不包含换行符，输出直接全部都是在同一行
                //方式二：直接添加bw.newLine()
                bw.write(data);
                bw.newLine();
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //关闭流
            try {
                if(br!=null) {
                    br.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

            try {
                if(bw!=null) {
                    bw.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
    /**
     * 图片的加密和解密,字节流
     */
    @Test
    public void test4()  {
        FileInputStream fis= null;
        FileOutputStream fos= null;
        try {
            //【注】其实可以直接写成FileInputStream fis=new FileInputStream("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\0.png");
            //会自动将字符串包装成一个File文件
            fis = new FileInputStream(new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\0.png"));
            fos = new FileOutputStream(new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\2.png"));

            byte[] buffer=new byte[20];
            int len;
            while((len=fis.read(buffer))!=-1){
                //需要提前对字节数组进行修改，加密
                //一次读取20个字节，需要循环一个一个字节修改
//                for(byte b:buffer){
//                    b=(byte)(b^5);
//                }          //错误的写法，增强for循环，没有修改源buffer中的数据
                //正确的写法
                for (int i = 0; i < len; i++) {
                    buffer[i]=(byte)(buffer[i]^5);//加密以后的文件与源文件大小相同，异或加密不改变源文件的大小，加密以后的文件不能打开
                }

                fos.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        finally {
            try {
                if(fis!=null) {
                    fis.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if(fos!=null) {
                    fos.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    /**
     * 图片的解密，字节流    m,n,    m^n^n=m
     */
    @Test
    public void test5(){
        /**
         * 解密只需要在加密的基础上，以加密文件作为原始文件再进行一次加密，就可以得到最初的解密文件
         */
    }

    /**
     * 获取文本上每个字符出现的次数
     * 遍历文本的每一个字符，字符及出现的次数保存在map中，将map中数据写入文件（map中内存中的数据）写入到磁盘中
     * map的key存放字符，value可以用来统计出现的次数
     */
    @Test
    public void test6() {
        FileReader fr= null;
        BufferedWriter bw= null;
        try {
            fr = new FileReader(new File("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\hello.txt"));
            bw = new BufferedWriter(new FileWriter("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\count.txt"));
            //创建map
            Map<Character, Integer> map = new HashMap<>();
            //遍历每一个字符，每一个字符出现的次数放到map中
            int c=0;
            while ((c = fr.read()) != -1) {
                //int还原char
                char ch=(char)c;
                if (map.get(ch) == null) {
                    map.put(ch,1);
                }else{
                    map.put(ch,map.get(ch)+1);
                }
            }
            //把map中数据存在文件count.txt
//        创建writer
            //遍历map，再写入数据
            Set<Map.Entry<Character,Integer>> entrySet=map.entrySet();
            for(Map.Entry<Character,Integer> entry:entrySet){
                switch(entry.getKey()){
                    case ' ':
                        bw.write("空格="+entry.getValue());
                        break;
                    case '\t':
                        bw.write("tab键="+entry.getValue());
                        break;
                    case '\r':
                        bw.write("回车="+entry.getValue());
                        break;
                    case '\n':
                        bw.write("换行="+entry.getValue());
                        break;
                    default:
                        bw.write(entry.getKey()+"="+entry.getValue());
                        break;
                }
                bw.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if(bw!=null) {
                    bw.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
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
## 转换流 （字符流）InputStreamReader OutputStreamWriter
**应用：可以实现文件编码方式转换 从utf-8编码方式转为gbk编码方式**
```
/**
     * 转换流
     * 文件读取，首先将文件通过字节流读入，然后将读入的字节流转换为输入的字符流，将文件读入程序
     * 文件写出：将文件从程序中通过字符流的形式输出，然后将字符流转换为字节流，将文件以字节流输出到磁盘中
     *
     * 1、转换流
     * InputStreamReader
     * OutputStreamWriter,   一个流是属于字符流还是字节流，应该看后缀，所以这两个流属于字符流
     *
     *解码：将字节，字节数组------>字符数组，字符串InputStreamReader
     * 编码：字符数组，字符串---------->字节、字节数组OutputStreamWriter
     *
     * 字符集
     */
    @Test
    public void test1() throws IOException {
        //此时处理异常的话，仍然应该使用try -catch-finally
        FileInputStream fis=new FileInputStream("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\hello.txt");
        InputStreamReader isr=new InputStreamReader(fis,"UTF-8");//如果不写字符集，就是用系统默认的字符集，IDEA设置的utf-8
        //第二个参数指定字符集，是根据存的时候采用的编码方式写的
        char[] cbuf=new char[20];
        int len;
        while((len=isr.read(cbuf))!=-1){
            String str=new String(cbuf,0,len);
            System.out.println(str);
        }

        isr.close();
    }
    /**
     * 综合使用InputStreamReader和OutputStreamWriter
     *应用：可以转换文件的编码方式 从utf-8编码的文件转换为gbk编码的文件
     */
    @Test
    public void test2() throws IOException {
        FileInputStream fis=new FileInputStream("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\hello.txt");

        FileOutputStream fos=new FileOutputStream("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\hello_gbk.txt");
        InputStreamReader isr=new InputStreamReader(fis,"UTF-8");
        OutputStreamWriter osw=new OutputStreamWriter(fos,"gbk");

        char[] cbuf=new char[20];
        int len;
        while((len=isr.read(cbuf))!=-1){
            osw.write(cbuf,0,len);
        }

        osw.close();
        isr.close();
//        fos.close();
//        fis.close();

    }
/**
 * 多种字符编码方式
 */
 ```
 ## 输入输出流  打印流（将流进行重定向） 数据流（对基本数据类型数据的读写操作）
 ```
  /**
     * (一)标准的输入输出流，
     * 标准输入流：键盘输入System.in   标准输出流：显示器、控制台输出System.out
     * System.in的类型是InputStream  System.out的类型是PrintStream,其是OutputStream的子类  FilterOutputStream的子类
     *可以通过System类的setIn(InputStream is)/setOut(PrintStream ps)方式重新指定输入输出流  重定向
     *
     * （二）打印流,都是输出流
     *PrintStream（打印字节输出流）;PrintWriter（打印字符输出流）
     * 实现将基本数据类型的数据格式转化为字符串输出
     *提供一系列的重载print()和println()
     *
     * （三）数据流
     *对基本数据类型操作
     */
//    @Test
//    public void test1()  {
            public static void main(String args[]){
        BufferedReader br= null;
        try {
            //从键盘输入字符串，要求将读取到的整行字符串转换成大写输出，然后继续进行输入操作，直至当输入e或者exit时，退出程序
            //方法一：使用scanner实现  next():返回字符串
            //方法二：System.in实现。System.in----》转换流----》BufferedReader的readLine()
            InputStreamReader isr=new InputStreamReader(System.in);
            br = new BufferedReader(isr);
            String data;
            while (true) {
                System.out.println("请输入字符串：");
                data=br.readLine();
                if("e".equalsIgnoreCase(data)||"exit".equalsIgnoreCase(data)){
                    System.out.println("程序结束");
                    break;
                }
                String s = data.toUpperCase();
                System.out.println(s);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if(br!=null) {
                    br.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 打印流  重定向：从控制台输出重定向到文件输出
     */
    @Test
    public void test2(){

        PrintStream ps= null;
        try {
            FileOutputStream fos=new FileOutputStream("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\printstream.txt");
            ps = new PrintStream(fos,true);
            //创建打印输出流，设置为自动刷新模式（写入换行符或字节'\n'都会刷新缓冲区）
            if(ps!=null){
                System.setOut(ps);//把标准输出流（控制台输出）改成文件
            }
            for(int i=0;i<=255;i++){
                System.out.print((char)i);
                if(i%50==0){
                    System.out.println();
                }
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } finally {
            if(ps!=null) {
                ps.close();
            }
        }
    }

    /**
     * 数据流
     * （1）DataInputStream (2)DataOutputStream  字节流
     *作用：用于读取或写出基本数据类型的变量或字符串
     * 注意：处理异常的话，仍然应该使用try-catch-finanly
     */
    @Test
    public void test3() throws IOException {
        DataOutputStream dos=new DataOutputStream(new FileOutputStream("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\datastream.txt"));
        dos.writeUTF("速度");
        dos.flush();//刷新操作，将数据写入文件
        dos.writeInt(23);
        dos.flush();
        dos.writeBoolean(true);
        dos.flush();

        dos.close();
    }
    /**
     * 读取数据流
     * 将文件中存储的基本数据类型变量和字符串读取到内存中，保存在变量中
     * 注意点：读取不同类型的数据顺序要与当初写入文件时，保存的数据顺序一致
     */
    @Test
    public void test4() throws IOException {
        DataInputStream dis = new DataInputStream(new FileInputStream("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\datastream.txt"));
        String name = dis.readUTF();
        int age = dis.readInt();
        boolean b = dis.readBoolean();//读取的顺序必须按照文件的写入顺序一致，否则会报错
        System.out.println("name:"+name+"age:"+age+"b:"+b);
        dis.close();
    }
```
### IDEA和Eclipse中相对路径的区别：
```
/**
     * 【注意】：
     * 相对路径在IDEA和eclipse中使用的区别：
     * IDEA:
     * 如果使用单元测试方法，相对路径基于当前的Moudle的
     * 如果使用main()测试，相对路径基于当前project的
     * 
     * Eclipse：
     * 单元测试方法还是main(),相对路径都是基于当前project的
     */
```



