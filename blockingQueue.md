# Java并发之BlockingQueue的使用

### 一.简介

　　前段时间看到有些朋友在网上发了一道面试题，题目的大意就是：有两个线程A,B,  A线程每200ms就生成一个[0,100]之间的随机数， B线程每2S中打印出A线程所产生的增量随机数。其实该题目笔者认为考察的知识点就是本博文要给大家介绍的BlockingQueue这个接口，对于该题目笔者认为考察的只是面试者对这个类是否掌握，别无其他。

二.BlockingQueue的使用

　　BlockingQueue是一个阻塞队列，用户可以为该队列设置一个初始容量（即该队列中最多能够放入多少个数据）。既然是队列，那么肯定是由顺序的，我们可以调用给定的API依次获取往该队列中值的设置顺序。关于API笔者在解决上述的面试题后，会挑一些重要的并且很常用的给读者作详细的说明。

　　以下代码实例就是笔者解决该面试题的代码，如果读者有更好的解决方式，欢迎给笔者留言，若有错误之处，请多多指教。

```java
public class BlockingQueueTest {
    /**
     * 实例化一个队列，队列中的容量为10
     */
    private static BlockingQueue<Integer> blockingQueue = new LinkedBlockingQueue<>(10);
    public static void main(String[] args) {
        ScheduledExecutorService product = Executors.newScheduledThreadPool(1);
        Random random = new Random();
        product.scheduleAtFixedRate(() -> {
            int value = random.nextInt(101);
            try{
                blockingQueue.offer(value);  //offer()方法就是网队列的尾部设置值
            }catch(Exception ex){
                ex.printStackTrace();
            }
        }, 0, 200, TimeUnit.MILLISECONDS);  //每100毫秒执行线程

        new Thread(() -> {
            while(true){
                try {
                    Thread.sleep(2000);
                    System.out.println("开始取值");
                    List<Integer> list = new LinkedList<>();
                    blockingQueue.drainTo(list);  //drainTo()将队列中的值全部从队列中移除，并赋值给对应集合
                    list.forEach(System.out::println);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}
```


三.BlockingQueue中API介绍

`offer(E e)`: 将给定的元素设置到队列中，如果设置成功返回true, 否则返回false. e的值不能为空，否则抛出空指针异常。

`offer(E e, long timeout, TimeUnit unit)`: 将给定元素在给定的时间内设置到队列中，如果设置成功返回true, 否则返回false.

`add(E e)`: 将给定元素设置到队列中，如果设置成功返回true, 否则抛出异常。如果是往限定了长度的队列中设置值，推荐使用offer()方法。

`put(E e)`: 将元素设置到队列中，如果队列中没有多余的空间，该方法会一直阻塞，直到队列中有多余的空间。

`take()`: 从队列中获取值，如果队列中没有值，线程会一直阻塞，直到队列中有值，并且该方法取得了该值。

`poll(long timeout, TimeUnit unit)`: 在给定的时间里，从队列中获取值，如果没有取到会抛出异常。

`remainingCapacity()`：获取队列中剩余的空间。

`remove(Object o)`: 从队列中移除指定的值。

`contains(Object o)`: 判断队列中是否拥有该值。

`drainTo(Collection c)`: 将队列中值，全部移除，并发设置到给定的集合中。
