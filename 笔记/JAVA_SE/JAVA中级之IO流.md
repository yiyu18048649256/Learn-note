### I/O流

#### 一.IO流中的结构

![图片](https://img-blog.csdn.net/20170523222107234?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvWXVlX0NoZW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![](https://img-blog.csdn.net/20170523222107234?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvWXVlX0NoZW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**字符流**：顾名思义，该流只能处理字符，但处理字符速度很快
**字节流**：可以处理所有以bit为单位储存的文件，也就是说可以处理所有的文件，但是在处理字符上的速度不如字符流

#### 二.IO流的具体使用

从各种输入流到各种输出流 
注：其实在各个不同的类型中，输入流到输出流的套路基本都一样。 
那就拿最简单的FileOutputStream来举例子吧 
从FileOutputStream到FileIntputStream其实就是复制一个文件的过程，将文件读取到FileIntputStream中，后输出到FileOutputStream也就是相当于输出到了硬盘的文件中。 
我们可以以两个桶为例，一个桶为FileIntputStream，另一个桶为FileOutputStream，如果要把一个桶里的水转移到另一个桶中，我们首先需要一个水瓢，一次次的舀水才能完成我们的需求。 
废话不多说，直接上代码：

```
public static void main(String[] args) throws IOException {
        File fil1 = new File("D:/111.pdf");
        File fil2 = new File("D:/222.pdf");
        try (FileInputStream fi = new FileInputStream(fil1); 
        //一个叫输入流的桶，装满了一桶叫做D:/111.pdf文件的水
        FileOutputStream fs = new FileOutputStream(fil2);
        //一个叫输出流的空桶，但想装满叫做"D:/222.pdf"文件的水
                ) {
            byte[] buf = new byte[521];
            //叫做buf的水瓢
            int len = -1;
            //用来测量每次水瓢装了多少水
            while((len = fi.read(buf)) != -1){
            //一次次的用水瓢在输入流的桶里舀水，并用len测了舀了多少水，当len等于-1意味着水舀光了，该结束舀水了。
                fs.write(buf, 0, len);
                //一次次把水瓢里的水放到了输出流的桶里
            }
            fs.flush();
        } catch (Exception e) {
        }
    }
```


其实这种方法可以针对于很多的输入流和输出流。 

- 从输入流到字符串 
  其实这个和上一种很类似，只不过换了种实现方式。 
  直接上代码：

        File file = new File("D:/123.txt");
        FileInputStream fis = new FileInputStream(file);
        //同样是叫做输入流的桶
        StringBuffer sb = new StringBuffer();
        //把输出流的桶换成了StringBuffer用来储存字符串
        //其实也可以直接用String，但是StringBuffer速度更快。
        byte[] buf = new byte[256];
        //水瓢没变
        int len = -1;
        //测水瓢舀了多少水没变
        while ((len = fis.read(buf)) != -1){
            sb.append(new String(buf, 0, buf.length));
            //和上面的原理基本一样，只不过换了个水瓢而已
            //new String(buf, 0, buf.length)是将buf里面的内容转换为字符串
        }
        System.out.println(sb.toString());
  使用字符流向文件中写入字符串或者在文件中读取字符串 
  其实和前面的字节流读写思路一样，只是限制了文件只能为字符类型的
  复制文本文件并输出

```
 public static void main(String[] args) throws Exception {
        File file = new File("D:/123.txt");
        //复制源文件
        File file2 = new File("D:/456.txt");
        //复制结果文件
        StringBuffer sb = new StringBuffer();
        //用于输出到控制台
        if(!file2.exists()){
            file2.createNewFile();
        }
        //检测结果文件是否存在如果不存在便创建一个
        FileReader fr = new FileReader(file);
        //设置字符读入流用于向文件（file）中读数据
        FileWriter fw = new FileWriter(file2);
        //设置字符读出流用于向文件（file2）中写数据
        char[] ch = new char[256];
        //每次读和写的容器，或者说是传送的媒介
        int len = -1;
        while((len = fr.read(ch)) != -1){
            fw.write(ch, 0, ch.length);
            //将容器里的东西写入到新文件中
            sb.append(new String(ch, 0, ch.length));
            //将容器里的东西添加到strngBuffer中，用于输出
        }
        fw.flush();
        fw.close();
        fr.close();
        System.out.println(sb.toString());
        //输出文本文件
    }
```

对对象进行序列化及反序列化 
使用工具：ObjectOutputStream，ObjectInputStream 
介绍：将对象以文件的形式保存在硬盘中，使之能更方便的传输。 
条件：必须实现Serializable接口（实现了这个接口，但并不需要重写任何方法）
代码：
```
class DemoObject implements Serializable{
    int date = 23; 
}

public class IoTest {
    public static void main(String[] args) throws Exception {
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(new File("D:/123.obj")));
        //建立对象输出流准备向文件中写入对象
        oos.writeObject(new DemoObject());
        //向文件中写入新建立的对象
        oos.flush();
        //输出流记得要flush
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(new File("D:/123.obj")));
        //建立对象输入流准备在文件中读出刚写入的对象
        DemoObject newObject = (DemoObject)ois.readObject();
        //建立一个新对象用于保存刚刚读出的对象
        System.out.println(newObject.date);
        //输出这个对象
    }
}
```