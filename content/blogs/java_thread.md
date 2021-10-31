Title: java 多线程的一些总结
Date: 2021-10-30 08:04
Category: java
Tags: java, thread
Authors: Fanson
Summary: 对java 多线程的一些总结

---------------------------------

### 多线程

1. 多线程改变了代码的执行方式，从串行操作该百年为多段代码之间并行执行，这种执行方式称之为”并发“
2. 多线程创建方式一：继承Thread并重写run方法来定义线程任务

   ```java
   public class ThreadDemo {
       public static void main(String[] args) {
           Thread t1 = new MyThread1();
           Thread t2 = new MyThread2();
           t1.start();
           t2.start();
       }
   }

   class MyThread1 extends Thread {
       public void run() {
           for (int i=0; i<1000; i++) {
               System.out.println("hello");
           }
       }
   }

   class MyThread2 extends Thread {
       public void run() {
           for (int i=0; i<1000; i++) {
               System.out.println("yes");
           }
       }
   }
   ```

3. 创建线程的第二种方式，实现Runnable接口单独定义任务

   ```java
   public class ThreadDemo {
       public static void main(String[] args) {
           Runnable r1 = new MyRunnable1();
           Runnable r2 = new MyRunnable2();

           Thread t1 = new Thread(r1);
           Thread t2 = new Thread(r2);
           
           t1.start();
           t2.start();
       }
   }

   class MyRunnable1 implements Runnable {
       public void run() {
           for (int i=0; i<1000; i++) {
               System.out.println("你是谁");
           }
       }
   }

   class MyRunnable2 implements Runnable {
       public void run() {
           for (int i=0; i<1000; i++) {
               System.out.println("我是");
           }
       }
   }
   ```

4. 使用匿名内部类完成两种线程的创建

   ```java
   public class ThreadDemo {
       public static void main(String[] args) {
           Thread t1 = new Thread() {
               public void run() {
                   for (int i=0; i<1000; i++) {
                       System.out.println("who are you");
                   }
               }
           };

           Runnable r2 = new Runnable() {
               public void run() {
                   for (int i=0; i<1000; i++) {
                       System.out.println("i'm mararo");
                   }
               }
           };
           Thread t2 = new Thread(r2);

           t1.start();
           t2.start();
       }
   }

5. 线程提供了一个静态方法**Thread currentThread()**该方法可以取运行这个方法的线程

   ```java
   /**
     * java中所有代码都是靠线程运行的，main方法也不例外
     * 运行main方法的线程称为主线程，也是我们程序的第一条线程
     */
   public class CurrentThreadDemo {
       public static void main(String[] args) {
           Thread main = Thread.currentThread();
           System.out.println("主线程"+main);

           Thread t = new Thread() {
               public void run() {
                   Thread t = Thread.currentThread();
                   // 获取线程信息
                   String name = t.getName();
                   long id = t.getId();
                   int pri = t.getPriority();
                   boolean isAlivce = t.isAlive();
                   boolean isDaemon = t.isDaemon();
                   boolean isInter = t.isInterrupt();
                   System.out.println("自定义线程" + t);
               }
           }
           t.start();
       }
   }
   ```

6. 线程优先级

    线程有10个优先级，分别用整数1-10表示，  
    其中1为最低优先级，5为默认优先级，10最高  
    优先级越高的线程获取的CPU时间片次数越多

   ```java
   public class PriorityDemo {
       public static void main(String[] args) {

           Thread min = new Thread() {
               public void run() {
                   for (int i=0; i<1000; i++) {
                       System.out.println("min");
                   }
               }
           };

           Thread norm = new Thread() {
               public void run() {
                   for (i=0; i<1000; i++) {
                       System.out.println("norm");
                   }
               }
           };

           Thread max = new Thread() {
               public void run() {
                   for (i=0; i<1000; i++) {
                       System.out.println("max");
                   }
               }
           };

           min.setPriority(Thread.MIN_PRIORITY);
           max.setPriority(Thread.MAX_PRIORITY);

           min.start();
           norm.start();
           max.start();
       }
   }
   ```

7. sleep阻塞
   线程提供了一个静态方法
   static void sleep(long ms)
   该方法可以让这个运行这个方法的线程处于阻塞状态
   指定毫秒， 超时后线程会自动回到RUNNABLE的状态
   等待再次分配时间片并运行

   ```java
   public class SleepDemo {
       public static void main(String[] args) {
           Scanner scanner = new Scanner(System.in);
           System.out.println("请输入一个额数字");
           int num = scanner.nextInt();
           for (; num>0; num--) {
               try {
                   System.out.println(num);
                   Thread.sleep(1000);
               } catch (InterruptedException e) {
                   e.printStackTrace();
               }
           }
       }
   }
   ```

   sleep方法要求我们处理中断异常，
   当一个线程调用sleep处于阻塞状态的过程中
   这个携程的interrupt方法被调用时，此时sleep方法
   就会抛出中断异常来强制打断睡眠阻塞

   ```java
   public class SleepDemo {
       public static void main(String[] args) {
           Thread lin = new Thread() {
               public void run() {
                   System.out.println("sleep a while");
                   try {
                       Thread.sleep(100000);
                   } catch (InterruptedException e) {
                       System.out.println("what happend");
                   }
                   System.out.println("wake up");
               }
           };

           Thread huang = new Thread() {
               public void run() {
                   System.out.println("start to crash");
                   for (int i=0; i<5; i++) {
                       System.out.println("80U");
                       try {
                           Thread.sleep(1000);
                       } catch (InterruptedException e) {
                       }
                       System.out.println("done");
                       lin.interrupt();
                   }
               }
           };

           lin.start();
           huang.start();
       }
   }
   ```

8. 守护线程

   守护线程也称为后台线程，默认创建的线程都是普通线程  
   通过设置，可以将一个普通线程变成守护线程  
   守护线程在创建与使用上无区别，但是在结束时机上有一个区别  
   > 当一个进程中所有的普通线程都结束时，进程就会结束，  
   此时该进程中的所有在运行的守护线程都会被强制杀死

   ```java
   public Class DaemonThreadDemo {
       public static void main(String[] args) {

           Thread rose = new Thread() {
               public void run() {
                   for (int i=0; i<5; i++) {
                       System.out.println("roes: just let me go");
                       try {
                           Thread.sleep(1000);
                       } catch (InterruptedException e) {
                       }
                   }

               }
           };

           Thread jack = new Thread() {
               public void run() {
                   while (true) {
                       System.out.println("jack:you jump! i jump!");
                       try {
                           Thread.sleep(1000);
                       } catch (InterruptedException e) {}
                   }
               }
           };

           rose.start();
           jack.setDaemon(true);
           jack.start();
       }
   }
   ```

9. join()

    该方法允许调用方法的线程阻塞，直到该方法所属的线程结束时解除阻塞状态  
    使用此方法可以协调线程间的同步运行

    同步运行：运行有先后顺序
    一部运行；多线程并发

    ```java
    public class JoinDemo {
        public static boolean isFinish = false;

        public static void main(String[] args) {

            Thread download = new Thread() {
                public void run() {
                    System.out.println("down:开始下载图片...");
                    for (int i=1; i<=100; i++) {
                        System.out.println("down:" + i + "%");
                        try {
                            Thread.sleep(50);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    System.out.println("down: 图片下载完毕！");
                }
            };

            Thread show = new Thread() {
                public void run() {
                    try {
                        System.out.println("show:开始显示文字");
                        Thread.sleep(3000);
                        System.out.println("show:显示文字完毕");

                        // 等待download下载完毕
                        download.join();

                        System.out.println("show:开始显示图片");
                        if (!isFinish) {
                            throw new RuntimeException("图片加载失败");
                        }
                        System.out.println("show:显示图片完毕");
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            };

            show.start();
            download.start();
        }
    }
    ```

10. 多线程并发安全问题

    当多个线程并发操作同一临界资源时，由于线程切换时机不确定，  
    导致操作代码执行顺序为按照设计意图执行所导致的操作混乱，
    严重时可能导致系统瘫痪  
    临界资源： 同一时间只允许一条线程操作的资源  

    ```java
    public class SyncDemo {
        public static void main(String[] args) {
            Table table = new Table();

            Thread t1 = new Thread() {
                public void run() {
                    while(true) {
                        int bean = table.getBean();
                        Thread.yield();
                        System.out.println(getName() + ":" + bean);
                    }
                }
            };

            Thread t2 = new Thread() {
                public void run() {
                    while (true) {
                        int bean = table.getBean();
                        Thread.yield();
                        System.out.println(getName() + ":" + bean);
                    }
                }
            };

            t1.start();
            t2.start();
        }
    }

    class Table {
        private int beans = 20;

        /**
          * 当一个方法被synchronized修饰后，该方法称为同步方法，
          * 即：多个线程不同同时在方法内部执行，将并发操作改为同步操作
          * 有效解决对线程的并发安全问题
          */
        public synchronized int getBean() {
            if (bean==0) {
                throw new RuntimeException("没有豆子了");
            }

            Thread.yield();
            return beans--;
        }
    }
    ```

11. 有效的缩小同步范围可以在保证并发安全的前提下，提高并发效率

    同步块：  
    synchronized(同步监视器对象){需要同步运行的代码片段}  

    ```java
    public class SyncDemo {
        public static void main(String[] args) {
            Shop shop = new Shop();

            Thread t1 = new Thread() {
                public void run() {
                    shop.buy();
                }
            };

            Thread t2 = new Thread() {
                public void run() {
                    shop.buy();
                }
            };

            t1.start();
            t2.start()
        }
    }

    public class Shop {

        public void buy() {
            try {
                Thread t = Thread.currentThread();
                System.out.println(t.getName() + "正在挑衣服");
                Thread.sleep(5000);

                /**
                  * 使用同步块，可以更精准的控制需要排队的代码片段，
                  * 但同步块要求必须指定同步监视对象，该对象可以是java中
                  * 任何类的实例，只要保证多个需要同步执行该代码片段的线程
                  * 看到的都是“同一个”即可
                  */
                synchronized (this) {
                    System.out.println(t.getName() + "正在试衣服...");
                    Thread.sleep(5000);
                }

                System.out.println(t.getName() + "结账离开");
            } catch (Exception e) {}
        }
    }
    ```

12. 静态方法上使用synchronized后，该方法一定具有同步效果

    静态方法指定的锁对象，为当前类的类对象的实例
    每个被JVM加载的类都有且只有唯一的一个Class实例
    因此，静态方法锁该对象就一定具备同步效果

    ```java
    public class SyncDemo {
        public static void main(String[] args) {
            Thread t1 = new Thread() {
                public void run() {
                    Foo.dosome();
                }
            };

            Thread t2 = new Thread() {
                public void run() {
                    Foo.dosome();
                }
            };

            t1.start();
            t2.start();
        }
    }

    public class Foo {
        public static void dosome() {
            /**
              * 静态方法中使用同步块时，指定的同步监视器对象依然选取
              * 当前类的类对象。
              * 获取方式为：类名.class
              */
            synchronized (Foo.class) {
                try {
                    Thread t = Thread.currentThread();
                    System.out.println(t.getName() + "正在执行dosome方法");
                    Thread.sleep(5000);
                    System.out.println(t.getName() + "执行dosome方法完毕");
                } catch (Exception e) {}
            }
        }
    }

13. 互斥锁

    当使用synchronized锁定多个代码片段，并且
    指定的都是同一个同步监视器对象时，这些代码片段
    之间就是互斥的，多个线程不能同时在这些代码间运行

    ```java
    public class SyncDemo {
        public static void main(String[] args) {
            Boo boo = new Boo();

            Thread t1 = new Thread() {
                public void run() {
                    boo.methodA();
                }
            };

            Thread t2 = new Thread() {
                public void run() {
                    boo.methodB();
                }
            };

            t1.start();
            t2.start();

        }
    }

    public class Boo {
        public synchronized void methodA() {
            try {
                Thread t = Thread.currentThread();
                Thread.sleep(5000);
            } catch (Exception e) {}
            
        }

        public synchronized void methodB() {
            try {
                Thread t = Thread.currentThread();
                Thread.sleep(5000);
            } catch (Exception e) {}
        }
    }
    ```

14. 线程池

    线程池主要解决两个问题：
    1. 线程的重用： 线程的生命周期不应当与任务一致，否则可能出现线程频繁的创建与销毁，给系统带来不必要的开销
    2. 控制线程的数量：一台计算机能承受的线程数量是有一定限度的

    ```java
    public class ThreadPoolDemo {
        public static void main(String[] args) {
            
            ExcutorService threadPool = Executors.newFixedThreadPool(2);

            for (int i=0; i<5; i++) {
                Runnable r = new Runnable() {
                    public void run() {
                        try {
                            Thread t = Thread.currentThread();
                            System.out.println(t + "正在执行任务");
                            Thread.sleep(5000);
                            System.out.println(t + "执行任务结束");
                        } catch (Exception e) {

                        }
                    }
                };

                // 将任务指派给线程池
                threadPool.execute(r);
            }

            threadPool.shutdownNow(r);
            System.out.println("线程池关闭了！");
        }
    }
    ```

