# java网络编程-实现无痛的网络连接
联网的底层细节被隐藏在java的本机安装系统里，由jvm进行控制。并且java实现类一个跨平台的网络库，面对的是一个统一的网络编程环境。
## InetAddress对象
```
public static void main(String[] args) throws UnknownHostException {
        InetAddress inet1 = InetAddress.getByName("192.168.56.10");  //根据ip地址或域名获得InetAddress对象
        System.out.println(inet1);
        //通过域名获得
        InetAddress inet2 = InetAddress.getByName("www.atguigu.com");
        System.out.println(inet2);

        InetAddress inet3 = InetAddress.getByName("127.0.0.1");
        System.out.println(inet3);
        //获取本机ip
        InetAddress inet4 = InetAddress.getLocalHost();//获得局域网内的ip
        System.out.println(inet4);

        //getHostName()
        System.out.println(inet1.getHostName());//就是getByName()的逆向操作，根据InetAddress对象获得ip地址和域名
        // getHostAddress()
        System.out.println(inet1.getHostAddress());
    }
```

## TCP实现客户端发送字符串给服务端
```
@Test
    public void client() {
        Socket socket= null;
        OutputStream os= null;
        try {
            InetAddress inet=InetAddress.getByName("127.0.0.1");
            socket = new Socket(inet,45455);
            os = socket.getOutputStream();
            os.write("你好呀，我是gg".getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(os!=null) {
                try {
                    os.close();  //关闭流，最好通过try catch finaly
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(socket!=null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    @Test
    public void server(){
        ServerSocket ss = null;
        Socket socket = null;
        InputStream inputStream = null;
        ByteArrayOutputStream baos=null;
        try {
            ss = new ServerSocket(45455);
            socket = ss.accept();
            inputStream = socket.getInputStream();

//            byte[] buffer=new byte[20];  //可能会出现乱码
//            int len;
//            while((len=inputStream.read(buffer))!=-1){
//                String str=new String(buffer,0,len);
//                System.out.println(str);
//            }

             baos=new ByteArrayOutputStream();
            byte[] buffer=new byte[5];
            int len;
            while((len=inputStream.read(buffer))!=-1){
                baos.write(buffer,0,len);//数据写入到类ByteArrayOutputStream中的数组中

            }
            System.out.println(baos.toString());

            System.out.println("收到了来自于："+socket.getInetAddress().getHostAddress()+"的数据");

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try { if(inputStream!=null) { inputStream.close(); } } catch (IOException e) { e.printStackTrace(); }
            try { if(socket!=null) { socket.close(); } } catch (IOException e) { e.printStackTrace(); }
            try { if(ss!=null) { ss.close(); } } catch (IOException e) { e.printStackTrace(); }
            try { if(baos!=null) { baos.close(); } } catch (IOException e) { e.printStackTrace(); }
        }
    }
```
## TCP实现客户端发送图片给服务端
```
 @Test
    public void client(){
        Socket socket = null;
        OutputStream os = null;
        FileInputStream fis= null;
        try {
            socket = new Socket(InetAddress.getByName("127.0.0.1"), 9090);
            os = socket.getOutputStream();
            fis = new FileInputStream("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\IOStream\\0.png");

            byte[] buffer=new byte[20];
            int len;
            while((len=fis.read(buffer))!=-1){
                os.write(buffer,0,len);

            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(fis!=null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(os!=null) {
                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(socket!=null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    @Test
    public void server() {

        FileOutputStream fos= null;
        ServerSocket ss= null;
        Socket socket = null;
        InputStream is = null;
        try {
            ss = new ServerSocket(9090);
            socket = ss.accept();
            is = socket.getInputStream();

            fos = new FileOutputStream("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\network\\1.png");

            byte[] buffer=new byte[20];
            int len;
            while((len=is.read(buffer))!=-1){
                fos.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(is!=null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(socket!=null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(ss!=null) {
                try {
                    ss.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(fos!=null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```
## TCP实现客户端发送图片文件给服务端，然后服务端响应一段消息给客户端
```
 @Test
    public void client()  {
        Socket socket= null;
        OutputStream os = null;
        FileInputStream fis= null;
        try {
            socket = new Socket(InetAddress.getByName("127.0.0.1"),9090);

            os = socket.getOutputStream();

            fis = new FileInputStream("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\network\\1.png");

            byte[] buffer=new byte[20];
            int len;
            while((len=fis.read(buffer))!=-1){
                os.write(buffer,0,len);
            }

            //关闭数据传输
            socket.shutdownOutput();

            //接收来自服务器端的反馈信息，并输出到控制端
            InputStream inputStream = socket.getInputStream();

            ByteArrayOutputStream baos=new ByteArrayOutputStream();
            byte[] buffer1=new byte[20];
            int len1;
            while ((len1 = inputStream.read(buffer1)) != -1) {
                baos.write(buffer1,0,len1);
            }
            System.out.println(baos.toString());


        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(os!=null) {
                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(fis!=null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(socket!=null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    @Test
    public void server()  {
        ServerSocket ss= null;
        Socket socket = null;
        InputStream is = null;
        FileOutputStream fos= null;
        try {
            ss = new ServerSocket(9090);
            socket = ss.accept();
            is = socket.getInputStream();

            //保存文件
            fos = new FileOutputStream("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\network\\2.png");
            byte[] buffer=new byte[20];
            int len;
            while ((len = is.read(buffer)) != -1) {
                fos.write(buffer,0,len);
            }

            System.out.println("为什么不终止");

            //服务器端发送消息给客户端反馈收到消息，双向通信
            OutputStream outputStream = socket.getOutputStream();
            outputStream.write("我已经收到了数据".getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(fos!=null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(is!=null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(socket!=null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(ss!=null) {
                try {
                    ss.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

        }
    }
```
## UDP传输
+ UDP传输可以先启动客户端，不会报错，TCP如果先启动客户端，客户端不能连接到服务端会直接报错
```
 //发送端
    @Test
    public void sender() throws IOException {
        DatagramSocket socket=new DatagramSocket();

        String str="我是UDP方式发送的导弹";
        byte[] data = str.getBytes();
        InetAddress inet=InetAddress.getByName("127.0.0.1");
        DatagramPacket packet=new DatagramPacket(data,0,data.length,inet,9090);

        socket.send(packet);

        socket.close();
    }

    @Test
    public void receiver() throws IOException {
        DatagramSocket socket=new DatagramSocket(9090);

        byte[] buffer=new byte[100];
        DatagramPacket packet = new DatagramPacket(buffer,0,buffer.length);
        socket.receive(packet);

        System.out.println(new String(packet.getData(),0,packet.getLength()));
        socket.close();
    }
```
## URL编程
```
/**
     * URL网络编程
     */

    public static void main(String[] args) throws MalformedURLException {
        URL url=new URL("https://www.bilibili.com/video/BV1Kb411W75N?p=629");
        //获取该URL的协议名
        System.out.println(url.getProtocol());
        //获取该URL的主机名
        System.out.println(url.getHost());
        //获取该URL的端口号
        System.out.println(url.getPort());
        //获取该URL的文件路径
        System.out.println(url.getPath());
        //获取该URL的文件名
        System.out.println(url.getFile());
        //获取该URL的查询名
        System.out.println(url.getQuery());
    }

    /**
     * 获取网络上的图片，URL 获得一个url对象，该url都西昂openConnection(),获得一个openConnection对象，
     * 通过openConnection对象可以获得一个connect
     * 通过openConnection可以获得一个输入字节流
     */
    @Test
    public void test1()  {
        HttpURLConnection urlConnection = null;
        InputStream is = null;
        FileOutputStream fos= null;
        try {
            URL url=new URL("https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg.wang1314.net%2Fuploadfile%2F2016%2F2016-07-04%2F1467617080934u_249026_uw_600_wh_441_hl_18233_l.jpg&refer=http%3A%2F%2Fimg.wang1314.net&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1622038171&t=4cfdc8861ab3b290568036107a4f41a4");

            urlConnection = (HttpURLConnection)url.openConnection();

            urlConnection.connect();

            is = urlConnection.getInputStream();

            fos = new FileOutputStream("src\\main\\java\\com\\atguigu\\gulimall\\search\\thread\\network\\3.png");

            byte[] buffer=new byte[1024];
            int len;
            while((len=is.read(buffer))!=-1){
                fos.write(buffer,0,len);
            }

            System.out.println("下载完成");
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(is!=null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(fos!=null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(urlConnection!=null) {
                try {
                    urlConnection.connect();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```
