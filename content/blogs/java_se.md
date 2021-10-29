Title: java se的一些总结
Date: 2021-10-26 20:45
Category: java
Tags: java, se
Authors: Fanson
Summary: 对java se 的一些总结


---------------------------------
### String类型: String是一个不变对象，一旦创建内容不可改变

>  int -> str.length()  
>  char -> str.charAt(int i)   
>  int -> str.indexOf(String s)  
>  String -> str.startsWith(String s)  str.endsWith(String s)   
>  String -> str.substring(int start, int end)  
>  String -> str.toUpperCase()  str.toLowerCase()  
>  String -> str.trim() : 去除当前字符串两边的空白字符  
>  String -> String.valueOf(其他类型 param)  
1. boolean str.matches(String regex)  
```java
public class MatchesDemo {
    public static void main(String[] args) {
        String mail = "fanson@top.com";
        String regex = "[a-zA-Z0-9]+@[a-zA-Z0-9]+(\\.[a-zA-Z]+)+";
        boolean match = mail.matches(regex);
        if (match) {
            System.out.println("is mail");
        } else {
            System.out.println("not mail");
        }
    }
}
```
2. String str.replaceAll(String regex, String str)
```java
public class ReplaceAllDemo {
    public static void main(String[] args) {
        String str = "abc123afe32432dgdsg";

        // 将字符串中的数字部分用“#NUMBER”代替
        str = str.replaceAll("[0-9]+", "#NUMBER#");
    }
}
```
3. String[] str.split(String regex)
```java
public class SpliteDemo {
    public static void main(String[] args) {
        String str = "abc123dfe232fgr";
        
        // 按照数字部分查分，保留字母部分
        String[] data = str.split("[0-9]+");
    }
}
```
4. java.lang.StringBuilder
> 该类是专门用来修改字符串的API，其内部维护一个可变的char数组，因此不会出现修改一次就创建一个新对象的问题，并且StringBuilder还提供了便于修改字符串内容的：增删改查操作
```java
public class StringBuilderDemo {
    public static void main(String[] args) {
        // 默认实例化出来一个表示空字符串
        StringBuilder builder = new StringBuilder();

        String str = "java this is";
        StringBuilder builderPlus = new StringBuilder(str);

        builder.append("test");
        str = builder.toString();

        builder.replace(1, 3, "this is repalce");

        builder.delete(0, 3);

        builder.insert(1, "inert");
    }
}

```

---------------------------------
### Interger
> 包装类是为了解决基本类型不能直接参与面向对象开发而诞生的，就是让基本类型可以以“对象”的形式存在
```java
public class IntegerDemo {
    public static void main(String[] args) {
        int a = 128;

        // java 建议使用value转换
        Integer i = Integer.valueOf(a);

        // 包装类转换为基本类型
        int d = i.intValue();

        // 所有数字类型的包装类，都有两个常量，MAX_VALUE, MIN_VALUE
        int max = Integer.MAX_VALUE;
        int min = Integer.MIN_VALUE;
    }
}
```
> 包装类有一个非常实用的功能，就是将字符串解析为对应的基本类型，前提，该字符串的内容正确的描述了基本类型可以保存的值
```java
public class IntegerDemo {
    public static void main(String[] args) {
        String str = "123";
        int d = Integer.parseInt(str);
        double dou = Double.parseDouble(str);
    }
}
```

---------------------------------
### 文件
1. File用来表示文件系统中的一个文件或目录
   1. 访问其表示的文件或目录的属性（名称，大小）
   2. 操作文件或目录
   3. 访问目录子项
   4. 但不能访问文件数据
```java
public class FileDemo {
    public static void main(String[] args) {
        File file = new File("./demo.txt");

        String name = file.getName();
        long length = file.length();

        boolean cr = file.canRead();  // 可读
        boolean cw = file.canWrite(); // 可写
        boolean ih = file.isHidden(); // 隐藏
    }
}
```
2. 使用File创建一个新文件
```java
public class CreateNewFileDemo {
    public static void main(String[] args) throws IOException {
        File file = new File("./text.txt");
        if (!file.exists()) {
            file.createNewFile();
        } else {
            // 文件存在
        }
    }
}
```
3. 使用File删除一个文件
```java
public class DeleteFileDemo {
    public static void main(String[] args) {
        File file = new File("./text.txt");
        if (file.exists()) {
            file.delete()
        }
    }
}
```
4. 创建一个空目录
```java
public class MKdirDemo {
    public static void main(String[] args) {
        File dir = new File("./demo");
        if (!dir.exists()) {
            dir.mkdir();
        }
    }
}
```
5. 创建多级目录
```java
public class MkdirsDemo {
    public static void main(String[] args) {
        File dir = new File("./a/b/c/d/e/f");
        if (!dir.exists()) {
            dir.mkdirs();
        }
    }
}
```
6. 删除目录
```java
public class DeleteDemo {
    public static void main(String[] args) {
        File dir = new File("./demo");
        if (dir.exists()) {
            // 使用delete方法删除目录时要求该目录必须是一个空目录，否则删除失败
            dir.delete();
        }
    }
}
```
7. 获取一个目录的所有子项
```java
public class ListFilesDemo {
    public static void main(String[] args) {
        File dir = new File(".");

        /**
          * boolean isFile()
          * boolean isDirectory()
          */
        if (dir.isDirectory()) {
            File[] subFiles = dir.listFiles();
        }
    }
}
```

> File提供了一个重载的listFiles， File[] listFiles(FileFilter filter)
```java
public class ListFilesDemo {
    public static void main(String[] args) {
        File dir = new File(".");

        if (dir.isDirectory()) {

            FileFilter filter = new FileFilter() {
                public boolean accept(File file) {
                    String fileName = file.getName();
                    return fileName.startsWith(".");
                }
            };

            File[] subFiles = dir.listFiles(filter);
        }
    }
}
```

---------------------------------
### RandomAccessFile
> 创建RAF两种构造方法：  
> RandomAccessFile(File f, String mode);  
> RandomAccessFile(String path, String mode);  
> "r": 只读模式
> "rw": 读写模式，对文件数据可读可写
1. 复制文件
```java
public class CopyDemo {
    public static void main(String[] args) throws IOException {
        RandomAccessFile src = new RandomAccessFile("./movie.wmv", "r");
        RandomAccessFile des = new RandomAccessFile("./movie_cp.wmv", "rw");

        int d = -1;
        while ((d=src.read()) != -1) {
            des.write(d);
        }

        src.close();
        des.close();
    }
}
```

2. 块读写
```java
/**
  * int read(byte[] data) 
  * 一次性读取给定字节数组总长度的字节量，并装入数组中，返回值为实际读取到的字节量
  * 如果，返回值为-1,则表示读取到了文件末尾
  *
  * void write(byte[] data)
  * 一次性将给定的字节数组中所有的字节写出
  *
  * void write(byte[] data, int offset, int len)
  * 将给定的字节数组从下标offset处连续len个字节一次性写出
  */
public class CopyDemo {
    public static void main(String[] args) throws IOException {
        RandomAccessFile src = new RandomAccessFile("./movie.wmv", "r");
        RandomAccessFile des = new RandomAccessFile("./movice_cp.wmv", "rw");

        byte[] data = new byte[1024*10];
        int len = -1 //每次实际读取的字节量

        while ((len=src.read(data)) != -1) {
            des.write(data, 0, len);
        }

        src.close();
        des.close();
    }
}
```

3. 简易记事本
```java
public class Note {
    public static void main(String[] args) throws IOException {
        Scanner scanner = new Scanner(System.in);

        String fileName = scanner.nextLine();

        RandomAccessFile raf = new RandomAccessFile(fileName, "rw");
        
        System.out.println("please start typing");
        while(true) {
            String line = scanner.nextLine();
            if ("exit".equals(line)) {
                break;
            }
            raf.write(line.getBytes("UTF-8"));
        }

        System.out.println("bye!");
        raf.close();
    }
}
```

4. 写入字节
```java
public class RAFDemo {
    public static void main(String[] args) throws IOException {
        RandomAccessFile raf = new RandomAccessFile("./raf.dat", "rw");

        /**
          * 将给定int值的对应的2进制的“低八位”写入文件
          * 00000000 00000000 00000000 00000001
          */
        raf.write(1);
        raf.close();
    }
}
```

5. RAF基于指针的操作
```java
public class RAFDemo {
    public static void main(String[] args) throws IOException {
        RandomAccessFile raf = new RandomAccessFile("test.dat", "rw");

        // 获取raf的指针位置
        long pos = raf.getFilePointer();

        // 写入一个int最大值
        int max = Integer.MAX_VALUE;
        raf.write(max>>>24);
        raf.write(max>>>16);
        raf.write(max>>>8);
        raf.write(max);

        // 将给定的值按照其字节大小一次性写入
        raf.writeInt(max);
        raf.writeLong(123L);
        raf.writeDouble(123.123);

        /**
          * void seek(long pos)
          * 移动指针到指定的位置
          */
        raf.seek(0);

        // 连续读取
        int d = raf.readInt();
        long l = raf.readLong();
        double dou = rad.readDouble();
    }
}
```

6. 读取文本数据
```java
public class ReadStringDemo {
    public static void main(String[] args) throws IOException {
        RandomAccessFile raf = new RandomAccessFile("./raf.txt", "r");

        byte[] data = new byte[(int) raf.length()];
        raf.read(data);

        String content = new String(data, "UTF-8");
        System.out.println(content);

        raf.close();
    }
}
```

7. 用户注册案例
```java
public class RegDemo {
    public static void main(String[] args) throws IOException {
        Scanner scanner = new Scanner(System.in);

        String username = scanner.nextLine();
        String password = scanner.nextLine();
        String nickname = scanner.nextLine();
        int age = scanner.nextInt();

        RandomAccessFile raf = new RandomAccessFile("user.dat", "rw");

        raf.seek(raf.length());

        byte[] data = username.getBytes("UTF-8");
        data = Arrays.copyOf(data, 32);
        raf.write(data);
        
        data = password.getBytes("UTF-8");
        data = Arrays.copyOf(data, 32)
        raf.write(data);

        data = nickname.getBytes("UTF-8");
        data = Arrays.copyOf(data, 32);
        raf.write(data);

        raf.writeInt(age);

        raf.close();
    }
}

public class ShowAllUserDemo {
    public static void main(String[] args) throws IOException {
        RandomAccessFile raf = new RandomAccessFile("user.dat", "r");

        for (int i=0; i<raf.length()/100; i++) {
            byte[] data = new byte[32];

            raf.read(data);
            String username = new String(data, "UTF-8").trim();

            raf.read(data);
            String password = new String(data, "UTF-8").trim();

            raf.read(data);
            String nickname = new String(data, "UTF-8").trim();

            int age = raf.readInt();

            raf.close();
        }
    }
}
```

8. 写入文本数据  
```java
public class WriteStringDemo {
    public static void main(String[] args) throws IOException {
        RandomAccessFile raf = new RandomAccessFile("./raf.txt", "rw");

        String str = "this is a test";
        byte[] data = str.getBytes("UTF-8");
        raf.write(data);

        raf.write("this is a test".getBytes("UTF-8"));
        raf.close();
    }
} 
```

---------------------------------
### java IO
> java IO 以标准化的操作对外界统一读写数据，并且将读与写操作按照方向划分：  
> 输入：从外界到程序方向，用来读取数据  
> 输出：从程序到外界方向，用来写入数据  

> java.io.InputStream, java.io.OutputStream 都是抽象类  

> java将流分为两大类  
> 节点流：又称为低级流，是实际连接数据源与程序的“管道”，负责实际读写数据的流，读写一定是建立在节点流基础上进行的  
> 处理流：不能独立存在，必须连接在其他流上，目的是，当数据“流经”当前流时，对其进行加工处理，简化我们在读写时对数据的相应操作  

1. 文件流：java.io.FileOutputStream  
   1. 常用的一对低级流实现类，用来连接文件，对文件进行读写操作，功能与RandomAccessFile一致
   2. 文件输出流的常用构造方法
       ```java
       FileOutputStream(File file);
       FIleOutpurStream(String path);
       // 以上两种构造方法创建的文件输出流为覆盖模式，若指定文件存在，先清除再写入 
       ```
       ```java
       FileOutputStream(File file, boolean append);
       FileOutputStream(String path, boolean append);
       //以上两种构造方法创建的文件流为追加模式
       ```
   3. 样例：
       ```java
       public class FOSDemo {
           public static void main(String[] args) throws IOException {
               FileOutputStream fos = new FileOutputStream("./fos.txt", true);

               String line = "this is a test";
               byte[] data = line.getBytes("UTF-8");

               fos.write(data)
               fos.close();
           }
       }
       ```

2. 文件输入流：java.io.FileInputStream
   1. RAF是给予指针的随即读写，读写方式更灵活，并且可以对文件部分内容覆盖进行编辑，而文件流则不行，文件流是基于java标准IO的读写方式，而IO的读写方式为顺序读写，只能向后读或写操作，不能回退
   2. 文件流可以借助流连接完成负责的数据读写操作，这一点RAF不容易做到
   3. 样例：
       ```java
       public class FISDemo {
           public static void main(String[] args) throws IOException {
               FileInputStream fis = new FileInputStream("./fos.txt");

               byte[] data = new byte[100];
               int len = fis.read(data);
               // 实际读取len字节

               String line = new String(data, 0, len, "GBK");
               fis.close();
           }
       }
       ```

3. 文件流复制操作
   ```java
   public class CopyDemo {
       public static void main(String[] args) throws IOException {
           FileInputStream fis = new FileInputStream("./text.txt");
           FileOutputStream fos = new FileOutputStream("./text_cp.txt");

           int len = -1;
           byte[] data = new byte[1024*10];
           while((len=fis.read(data)) != -1) {
               fos.write(data, 0, len)
           }
           fis.close();
           fos.close();
       }
   }
   ```

4. 缓冲流 java.io.BufferedOutputStream  java.io.BufferedInputStream
   >  缓冲字节输入流是一对高级流，在流连接中的作用是提高读写效率  
   ```java
   public class CopyDemo {
       public static void main(String[] args) {
           FileInputStream fis = new FileInputStream("test.mp3");
           BufferedInputStream bis = new BufferedInputStream(fis);

           FileOutputStream fos = new FileOutputStream("test_cp.mp3");
           BufferedOutputStream bos = new BufferedOutputStream(fos);

           int data = -1;
           while((data=bis.read()) != -1) {
               bos.write(data);
           }

           bis.close();
           bos.close();
       }
   }
   ```

5. 缓冲输出流的缓冲区问题
   ```java
   /**
     * void flush()
     * 该方法是OutputStream定义的方法，所有字节输出流都有该方法
     * 但是只有缓冲流的该方法有实际意义，作用是一次性将缓冲区
     * 已经缓存的数据写出。
     * 之所以所有字节输出流都有这个方法是因为连接应用中缓冲流通常
     * 不是终端流（直接被我们操作的流）
     */
   public class BOSFlushDemo {
       public static void main(String[] args) throws IOException {
           FileOutputStream fos = new FileOutputStream("bos.txt");
           BufferedOutputStream bos = new BufferedOutputStream(fos);

           String str = "this is a test";
           byte[] data = str.getBytes("utf-8");

           bos.write(data);
           bos.flush()
           bos.close();
       }
   }
   ```

6. 对象流 java.io.ObjectOutputStream  java.io.ObjectInputStream
   ```java
   /**
     * 对象流是一对高级流，作用是可以将java对象与字节相互转换
     */
   public class OOSDemo {
       public static void main(String[] args) throws IOException {
           String name = "xiaoming";
           int age = 18;
           String gender = "man";
           String[] otherInfo = {"student", "from sichuan", "good boy"};
           Person person = new Person(name, age, gender, otherInfo);

           FileOutputStream fos = new FileOutputStream("person.obj");
           ObjectOutputStream oos = new ObjectOutputStream(fos);
           oos.writeObject(person);
           oos.close();
       }
   }
   /**
     * void writeObject(Object obj)
     * 对象所属的类必须实现Serializable接口，否则出现异常
     * 
     *   对象序列化：将一个对象经过对象流写出时，对象流会按照其结果将对象转换为一组字节
     *   数据持久化：这足被序列化后的字节再经过文件流写入文件（写入磁盘）做长久保存的过程
     */

   @Getter
   @Setter
   public class Person implements Serializable {
       private static final logn serialVersionUID = 1L;
       private String name;
       private int age;
       private String gender;
       private transient String[] otherInfo;

       public Person(String name, int age, String gender, String[] otherInfo) {
           super();
           this.name = name;
           this.age = age;
           this.gender = gender;
           this.otherInfo = otherInfo;
       }

   }

   public class OISDemo {
       public static void main(String[] args) throws IOException, ClassNotFoundException {
           FileInputStream fis = new FileInputStream("person.obj")
           ObjectInputStream ois = new ObjectInputStream(fis);
           Perseon person = (Pserosn) ois.readObject();
           ois.close();
       }
   }
   ```

7. 字符流
   1. java将流按照读写单位分为字节流与字符流
      * 字节流： 以字节为单位，一次最少读写8位2进制
      * 字符流： 以字符为单位，实际读写字节量由指定的字符集与读写的字符数据有关
   2. java.io.Writer  java.io.Reader 是两个抽象类，是所有字符输出流与字符输入流的超类
   3. java.io.OutputStreamWriter  java.io.InputStreamReader 转换流，是字符流的一对实现类，是一对高级流
      * 实际开发中，我们在读写文本数据时，流连接中经常会使用到转换流，他们是流连接中重要一环，但是我们不会直接操作这对流
      * 称其为转换流，是因为，java中其他高级字符流都只能连接在其他字符流上，都不能直接连接字节流，但是这对转换流可以连接在字节流上，固得此名。
   ```java
   public class OSWDemo {
       public static void main(String[] args) throws IOException {
           FileOutputStream fos = new FileOutputStream("text.txt");
           
           /**
             * 转换流在创建时通过指定第二个参数来确定字符集，这样通过当前流写出的文本数据都会
             * 按照该字符集转换为字节后再做写出
             */
           OutputStreamWriter osw = new OutputStreamWriter(fos, "utf-8");
           osw.write("this is a test");
           osw.close();
       }
   }

   public class ISRDemo {
       public static void main(String[] args) throws IOException {
           FIleInputStream fis = new FielInputStream("test.txt");
           InputStreamReader isr = new InputStreamReader(fis);
           int d = -1;
           while( (d=isr.read()) != -1) {
               System.out.println((char) d)
           }
           isr.close();
       }
   }
   ```
   4. 一对缓冲字符输入与输出的流，java.io.BufferedWriter  java.io.BufferedReader
   5. java.io.PrintWriter 内部总是连接BufferedWriter作为缓冲加速使用，并且PW还支持自动刷新功能，实际开发比较常用
      1. new PrintWriter(bw, true)开启自动刷新功能，每当我们调用其println方法写出一行字符串后会自动flush
   
   ```java
   public class PWDemo {
       public static void main(String[] args) throws IOException {
           PrintWriter pw = new PrintWriter("pw.txt", "UTF-8");
           pw.println("this will write into pw.txt");
           pw.close();
       }
   }

   public class PWDemo2 {
       public static void main(String[] args) throws IOException {
           FileOutputStream fos = new FileOutputStream("pw.txt");
           OutputStreamWriter osw = new OutputStreamWriter(fos, "UTF-8");
           BufferedWriter bw = new BufferedWriter(osw);
           PrintWriter pw = new PrintWriter(bw);

           pw.println("this will write in pw.txt");
           pw.close();
       }
   }

   public class BRDemo {
       public static void main(String[] args) throws IOException {
           FileInputStream fis = new FileInputStream("text.txt");
           InputStreamReader isr = new InputStreamReader(fis);
           BufferedReader br = new BufferedReader(isr);

           String line = null;
           // readLine() 返回一行字符串，不包含\n
           while((line=br.readLine()) != null) {
               System.out.println(line)
           }
       }
   }
   ```
   6. 总结
   ![alt](/images/JAVA_IO.png)