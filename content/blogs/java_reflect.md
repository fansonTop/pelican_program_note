Title: java 反射
Date: 2021-10-31 14:17
Category: java
Tags: java, reflect
Authors: Fanson
Summary: 对java反射的一些总结

---------------------------------

### java反射机制

1. 反射时一种动态机制，允许我们在程序运行期间实例化对象，调用方法，操作属性
2. 反射可以提高代码的灵活性，但是会带来跟多的系统开销和较慢的运行效率
3. Class类：类的类对象
   * JVM加载一个类的时候会连同实例化一个Class的实例，勇气表示加载的这个类
   * 通过这个类的实例可以获取类名，方法，属性，构造器等等，可以调用他们
4. 反射的第一步就是获取要操作类的类对象
   * 类名.class： java中任何类都可以这样做，但是这种形式硬编码的形式获取，不灵活
   * Class.forName(String className)：通过Class的静态方法获取，这里参数为要加载类的完全类名  
        Class cls = Class.forName("java.lang.String")
   * 通过类加载器ClassLoader获取

    ```java
    public class ReflectDemo {
        public static void main(String[] args) {
            Class cls = Class.forName("java.lang.String");
            String name = cls.getName();
            Method[] methods = cls.getDeclareMethods();
        }
    }
    ```

5. 使用反射机制实例化对象

   ```java
   public class ReflectDemo {
       public static void main(String[] args) {
           Person p = new Person();
           List list = new ArrayList();

           // 加载要实例化的类的类对象
           Class cls = Class.forName("reflect.Person");

           // 通过类对象实例化，
           // Class提供了一个newInstance()方法，用来实例化其表示的类的对象
           // 前提时这个类要有无参构造方法，否则不能用这个方式实例化
           Object obj = cls.newInstance();
       }
   }
   ```

6. 使用反射调用方法

    ```java
    Class cls = Class.forName(className);
    Object object = cls.newInstance();

    // 无参方法
    Method method = cls.getMethod(methodName);
    method.invoke(object);

    // 有参方法
    Method method1 = cls.getMethod(methodName1, String.class, int.class);
    method1.invoke(object, "String参数", "int参数");

    // 调用私有方法
    Method method2 = cls.getDeclareMethod(methidName2);
    method2.setAccessible(true);
    method2.invoke(obj);
    ```

