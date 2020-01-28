# Java并发之Semaphore的使用

### 一.简介

　　今天突然发现，看着自己喜欢的球队发挥如此的棒，然后写着博客，这种感觉很爽。现在是半场时间，就趁着这个时间的空隙，说说Java并发包中另外一个重量级的类Semaphore，这个类从字面意义上理解是"信号量"。

　　那么什么是信号量呢？我用一种比较通俗的方式来跟大家解释一下，就是在该类初始化的时候，给定一个数字A，每个线程调用acquire()方法后，首先判断A是否大于0，如果大于0，就将A减去1，然后执行对应的线程，如果不大于0，那么就会阻塞，直到其他线程调用了release()方法，将A加上1，该线程可能有执行的机会。请各位读者原谅笔者用这种粗俗的方式去解释这个问题，而并非其他博文所用的专业性很强的"许可"之类的词语，我认为专业的东西是给专业的人看的，很很多人想要去学习一个他根本就从未涉足的东西，那么我们就不能用专业的术语去描述，如果他能够理解所谓的专业术语，那么他还有学习的必要吗？

### 二. Semaphore的使用

　　场景介绍：有一个停车场只有5个车位，现在有100辆车要去抢这个5个车位，理想情况下最多只有五辆车同时可以抢到车位，那么没有抢到车位的车只能等到，其他的车让出车位，才有机会去使用该车位。
  
  ```java
  public class CarPark {
    public static void main(String[] args) {
        //阻塞队列
        BlockingQueue<String> parks = new LinkedBlockingQueue<>(5);

        parks.offer("车位一");
        parks.offer("车位二");
        parks.offer("车位三");
        parks.offer("车位四");
        parks.offer("车位五");

        ExecutorService executorService = Executors.newCachedThreadPool();

        //如博文中所说的初始值为5， 专业的说法就是5个许可证,初始化5个许可
        Semaphore semaphore = new Semaphore(5);

        for (int i = 0; i < 100; i++) {
            final int num = i;
           executorService.execute(() -> {
                try {
                    /**
                     * 获取许可，首先判断semaphore内部的数字是否大于0，如果大于0，
                     * 才能获得许可，然后将初始值5减去1，线程才会接着去执行；如果没有
                     * 获得许可(原因是因为已经有5个线程获得到许可，semaphore内部的数字为0)，
                     * 线程会阻塞直到已经获得到许可的线程，调用release()方法，释放掉许可，
                     * 也就是将semaphore内部的数字加1，该线程才有可能获得许可。
                     */
                    semaphore.acquire();
                    /**
                     *  拿到许可后去抢车位，如果没有抢到一直阻塞
                     *  直到其他车辆归还车位抢到为止。
                     */
                    String park = parks.take();  
                    System.out.println("车辆【" + num + "】获取到: " + park);
                    Thread.sleep((long) Math.random() * 6000);
                    semaphore.release(); //线程释放掉许可，通俗来将就是将semaphore内部的数字加1
                    parks.offer(park);  //归还车位
                    System.out.println("车辆【" + no + "】离开 " + park);
                    System.out.println(Thread.currentTread().getName);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            });
        }
    }
}
  ```
  
  ### [参考链接](https://www.cnblogs.com/miller-zou/p/6978422.html)
