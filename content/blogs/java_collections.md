Title: java 集合
Date: 2021-10-30 21:39
Category: java
Tags: java, collections
Authors: Fanson
Summary: 对java集合的一些总结

---------------------------------

### java.util.Collection接口

1. Collection是所有集合的顶级接口，里面定义了所有集合都需要具备的功能。
2. 集合与数组一样，可以保存一组并且提供相应的操作方法，方便使用
3. Collection下面派生了两个常用的子接口：
   * java.util.List:线性表，特点是可以存放重复元素，并且有序
   * java.util.Set:不可重复集合
   * 元素是否重复是依靠元素自身aquals比较结果而定的
4. 样例1：

   ```java
   public class CollectionDemo {
       public static void main(String[] args) {
           Collection collection = new ArrayList();
           collection.add("one");
           collection.add("two");
           collection.add("three");

           // 返回集合元素个数
           int size = c.size();

           // 判断当前集合是否为空
           boolean isEmpty = collection.isEmpty();

           // 清空集合
           collection.clear();
       }
   }
   ```

5. 元素的equals方法影响着集合很多操作，比如：集合是根据equals方法判断是否为重复元素  
   还有判断包含某个元素,以及删除某个元素

   ```java
   public class CollectionDemo {
       public static void main(Stirng[] args) {
           Collection c = new ArrayList();
           c.add(new Point(1, 2));
           c.add(new Point(2, 3));
           c.add(new Point(5, 6));

           Point p = new Point(1, 2);

           boolean contains = c.contains(p);
           c.remove(p);
       }
   }
   ```

6. 集合仅能存放引用类型元素，并存放的是元素的引用（地址）
   如果存放基本类型，会转换成相应的包装类型进行保存。

7. 集合的操作

   ```java
   public class CollectionDemo {
       public static void main(String[] args) {
           Collection collection = new ArrayList();
           collection.add("java");
           collection.add("C++");
           collection.add("c");

           Collection collection2 = new HashSet();
           collection2.add("c#");
           collection2.add("java");
           collection2.add("android");

           /**
             * 将戈丁的集合中的所有院所添加到当前集合中，
             * 若当前集合为Set需要注意，重复元素不会被添加
             */
           collection.addAll(collection2);

           /**
             * 判断当前集合是否包含给定集合中的所有元素
             */
           boolean contains = collection.containsAll(collection2);

           /**
             * 删除当前集合中与给定集合中的共有元素
             */
           collection.removeAll(collection2);
       }
   }
   ```

8. 集合的遍历

   Collection为所有集合提供同一的遍历方式：迭代器模式

   Iterator iterator() 该方法可以获取一个用于遍历当前集合元素的迭代器
   java.util.Iterator迭代器接口：不同的集合都提供了一个实现了迭代器接口的迭代器实现类，并且通过iterator()返回，以用于遍历当前集合元素

   ```java
   public class CollectionDemo {
       public static void main(String[] args) {
           Collection collection = new ArrayList();
           collection.add("one");
           collection.add("#");
           collection.add("two");
           collection.add("#");
           collection.add("three");

           Iterator it = collection.iterator();

           while (it.hasNext()) {
               String str = (String) it.next();

               if ("#".equals(str)) {
                   it.remove();

                   /**
                     * 迭代器在遍历的过程中不允许通过集合的方法增删元素，会抛出异常
                     * collection.remove(str);
                     */
               }
           }
       }
   }
   ```

9. for循环的新用法

    ```java
    String[] array = {"one", "two", "String"};
    for (String str: array) {
        System.out.println(str);
    }
    ```

---------------------------------

### 泛型

1. 定义：泛型也称为参数化类型，是指我们在使用某个类时，指定其定义的某个属性，方法的参数或返回值的类型，这样可以提高灵活性；
2. 泛型的原型时Objct，实际上时让编译器将Object当作我们指定的类型看待，使得赋值时编译器会协助检查类型匹配以及获取值时自动做类型转换
3. 泛型在集合中应用最广泛，用来指定集合中元素的实际类型
4. 样例:

    ```java
    public class CollectionDemo {
        public static void main(String[] args) {
            Collection<String> collection = new ArrayList<String>();

            /**
              * 指定泛型后，编译器会检查实际传入的值的类型
              * 是否与指定的泛型匹配，不匹配编译不通过
              */
            collection.add("one");
            collection.add("two");

            // 迭代器也支持泛型，与其遍历的集合泛型一致即可
            Iterator<String> it = collection.iterator();
        }
    }
    ```

---------------------------------

### java.util.List接口

1. 线性表，时Collection下面常见的子接口，特点是可以放重复元素，并且有序，提供了一组通过下表操作元素的方法
2. 常见实现类有两种：
   * java.util.ArrayList:内部使用数组实现，查询性能更好，增删性能弱；
   * java.util.LinkedList:增删性能优于查询性能
3. 样例：

   ```java
   public class ListDemo {
       public static void main(String[] args) {
           List<String> list = new ArrayList<>();
           list.add("one");
           list.add("two");
           list.add("three");

           // 获取指定下标对应的元素
           String str = list.get(1);

           // 将给定元素设置到指定位置，返回值为该位置原有的元素
           String oldEelement = list.set(1, "2");

           // 将指定元素插入到指定位置
           list.add(2, "2");

           // 删除指定下标对应的元素并将其返回
           String old = list.remove();
       }
   }
   ```

4. List提供了获取子集的操作
   List subList(int start, int end),注意含头不含尾

   ```java
   public class ListDemo {
       public static void main(String[] args) {
           List<Integer> list = new ArrayList<>();
           for (int i=0; i<10; i++) {
               list.add(i);
           }

           List<Integer> subList = list.subList(3, 8);

           // 对子集元素操作就是对原集合操作
           subList.set(2, 9);
           list.subList(2, 9).clear();
       }
   }
   ```

5. 集合转化为数组 toArray方法

   ```java
   public class CollectionToArrayDemo {
       public static void main(String[] args) {
           collection.add("one");
           collection.add("two");
           collection.add("three");

           // 如果我们给定的数组可以可用，toArray会将元素存入其中，否则自动创建一个数组
           String array = collection.toArray(new String[collection.size()])
       }
   }
   ```

6. 数组转换为集合：工具类java.util.Array提供了一个静态方法asList

   ```java
   public class ArrayToListDemo {
       public static void main(String[] args) {
           String[] array = {"one", "two", "three"};

           List<String> list = Arrays.asList(array);

           // 从数组转换来的集合对他的元素做操作就是对原数组元素的操作
           list.set(1, "2");

           /**
             * 由于数组是定长的，因此对集合的增删元素操作都是不支持的
             */
           
           // 所有集合都之hi一个参数为Collection集合的鼓噪方法
           List<String> list2 = new ArrayList<>(list);
       }
   }
   ```

7. java.util.Collections 集合的工具类定义这一组静态方法，便于我们操作集合

   * sort方法是对List集合进行自然排序的，从小到大

        ```java
        public class SortListDemo {
            public static void main(String[] args) {
                List<Integer> list =  new ArrayList<>();
                Random random = new Random();
                for (int i=0; i<10; i++) {
                    list.add(random.nextInt(100));
                }

                Collections.sort(list);

                // 从大到小排序
                Collections.sort(list, new Comparator<Integer>() {
                    public int compare(Integet o1, Integet o2) {
                        return o2 - o1;
                    }
                })
            }
        }
        ```

    * 如果要求集合元素必须可以比较，需要实现Comparable接口

        ```java
        public class SortListDemo {
            public static void main(String[] args) {
                List<Point> list = new ArrayList<>();
                list.add(new Point(1, 2));
                list.add(new Point(1,3));
                list.add(new Point(2,4));
                list.add(new Point(6,9));
                list.add(new Point(7,8));

                /**
                  * Collections的重载方法sort方法要求除了要处理的List集合外
                  * 还要传入一个参数Comparable
                  */
                Collections.sort(
                    list, new Comparator<Point>() {
                        public int compare(Point o1, Point o2) {
                            int len1 = o1.getX() + o1.getY();
                            int len2 = o2.getX() + o2.getY();
                            return len1 -len2
                        }
                    }
                );
            }
        }
        ```

8. 队列：队列是经典的数据结构之一，可以保存一组元素，但是存取元素必须遵循先进先出原则

   * java.util.Queue队列接口  继承自Collection， java.util.LinkedList是其实现类

     ```java
     public class QueueDemo {
         public static void main(String[] args) {
             Queue<String> queue = new LinkedList<>();

             // 入队操作
             queue.offer("one");
             queue.offer("two");
             queue.offer("three");

             // 出队操作 获取并删除队首元素
             String str = queue.poll();

             while (queue.size() > 0) {
                 String e = queue.poll();
             }
         }
     }

9. 双端队列：队列两端都可以做出入队列操作的队列

    * java.util.Deque双端队列接口，继承自Queue， java.util.LinkedList是其实现类

      ```java
      public class DequeDemo {
          public static void main(String[] args) {
              Deque<String> deque = new LinkedList<>();
              deque.offer("one");
              deque.offer("two");
              deque.offer("three");

              // 从队首入队
              deque.offerFirst("four");

              // 从队尾入队
              deque.offerLast("five");

              deque.pollFirst();
              deque.pollLast();
             
          }
      }
      ```

10. 栈结构：栈可以保存一组元素，但是存取必须遵循先进后出的原则

    ```java
    public class StackDemo {
        public static void main(String[] args) {
            Deque<String> stack = new LinkedList<>();

            stack.push("one");
            stack.push("two");
            stack.push("three");
            stack.push("four");
            stack.push("five");

            stack.pop();
        }
    }
    ```

---------------------------------

### java.util.Map接口 查找表

1. Map体现的结构是一个多行两列的表格，key-value
2. Map总是根据key来获取对应的value，因此保存时总是成对出现
3. Map要求key不能重复
4. 实现类：java.util.HashMap;

    ```java
    public class MapDemo {
        public static void main(String[] args) {
            Map<String, Integer> map = new HashMap<>();

            /**
              * 当put的key的值在map中已经存在时，会替换换来的value
              * 并将原来的value返回
              */ 
            map.put("语文", 99);
            map.put("数学", 98);
            Integer value = map.put("语文", 90);


            /**
              * 若给定的key不存在则返回值为null
              */
            value = map.get("数学");
            value = map.get("体育");  // null

            /**
              * remove
              * 删除当前的Map中给定的key对应的value，并返回这个value
              */ 
            value = map.remove("数学");

            int size = map.size();
            boolean ck = map.containsKey("语文");
            boolean cv = map.contaonsValue(99);


        }
    }

5. Map的遍历：key， value， key-value

   ```java
   public class MapDemo {
       public static void main(String[] args) {
            Map<String, Integer> map = new HashMap<>();
            map.put("语文", 99);
            map.put("数学", 98);
            map.put("英语", 97);
            map.put("物理", 96);
            map.put("化学", 99);

            Set<String> keySet = map.keySet();
            for (String key: keySet) {
                System.out.println("key:" + key);
            }

            Set<Entry<String, Integet>> entrySet = map.entrySet();
            for (Entry<String, Integet> e: entrySet) {
                String key = e.getKey();
                Integer value = e.getValue();
                System.out.println(key + ":" + value);
            }

            Collection<Integer> values = map.valus();
            for (Integer value: values) {
                System.out.println("value" + value);
       }
   }
   ```

