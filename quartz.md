### 前言
在我们的生活中，有一些东西它需要按照某个固定时间或者某个时间间隔去周而复始的执行下去，比如每个月月底按时缴纳水电费，很多应用软件都会在每个月某个固定时间生成平台会员的各种报表等这些应用场景。

综上这些类似应用场景在Java技术领域有一个叫做Quartz的框架他就能帮我们轻松实现很多我们需要的场景，我们只需要把主要的精力放在业务代码上面即可。

要使用Quartz首先我们需要导入maven或者gradle坐标
```java
<!-- https://mvnrepository.com/artifact/org.quartz-scheduler/quartz -->
<dependency>
    <groupId>org.quartz-scheduler</groupId>
    <artifactId>quartz</artifactId>
    <version>2.3.2</version>
</dependency>

```
### 介绍
Quartz一个开源的任务管理项目，值得学习
官网：http://www.quartz-scheduler.org/

What is the Quartz Job Scheduling Library?

Quartz is a richly featured, open source job scheduling library that can be integrated within virtually any Java application - from the smallest stand-alone application to the largest e-commerce system. Quartz can be used to create simple or complex schedules for executing tens, hundreds, or even tens-of-thousands of jobs; jobs whose tasks are defined as standard Java components that may execute virtually anything you may program them to do. The Quartz Scheduler includes many enterprise-class features, such as support for JTA transactions and clustering.

### 定时器种类
Quartz 中五种类型的 Trigger：SimpleTrigger，CronTirgger，DateIntervalTrigger，NthIncludedDayTrigger和Calendar 类（ org.quartz.Calendar）。
最常用的：
SimpleTrigger：用来触发只需执行一次或者在给定时间触发并且重复N次且每次执行延迟一定时间的任务。
CronTrigger：按照日历触发，例如“每个周五”，每个月10日中午或者10：15分。

### 存储方式
RAMJobStore和JDBCJobStore
对比：

类型|优点|缺点
--|:--:|--:
RAMJobStore|不要外部数据库，配置容易，运行速度快|因为调度程序信息是存储在被分配给JVM的内存里面，所以，当应用程序停止运行时，所有调度信息将被丢失。另外因为存储到JVM内存里面，所以可以存储多少个Job和Trigger将会受到限制
JDBCJobStore|支持集群，因为所有的任务信息都会保存到数据库中，可以控制事物，还有就是如果应用服务器关闭或者重启，任务信息都不会丢失，并且可以恢复因服务器关闭或者重启而导致执行失败的任务|运行速度的快慢取决与连接数据库的快慢

### Demo
```java
public class MyJob implements Job{
	private static final Logger log = LoggerFactory.getLogger(MyJob.class);

	@Override
	public void execute(JobExecutionContext context)throws JobExecutionException {
		log.info("MyJob  is start ..................");
		
        //在这里写自己的业务逻辑代码，比如支付宝每天定时统计每个会员的余额宝日收益，或者每个月的其他理财收益等
        
		log.info("Hello quzrtz  "+
				new SimpleDateFormat("yyyy-MM-dd HH:mm:ss ").format(new Date()));
		
		log.info("MyJob  is end .....................");
	}
	
}
```
```java
public class QuartzTest{

	private static logger log = LoggerFactory.getLogger(QuartzTest.class);
    
   public static void main(String[] agres){
   
   //创建SchedulerFactory工厂
   SchedulerFactory schedulerFactory = new StdSchedulerFactory();
   //获取调度实例
	Scheduler scheduler = schedulerFactory.getScheduler();
   //创建jobDetail
   JobDetail jobDetail = JobBuilder.newJob(MyJob.class).withDescription("athis is a Job").withIdentity("job1_1", "jGroup1").build();
   
   //任务开始运行时间
   long time= LocalDateTime.now().toEpochSecond(ZoneOffset.of("+8")) + 3 * 3000;
   // 2、创建Trigger
   CronScheduleBuilder builder = CronScheduleBuilder.cronSchedule("0/2 * * * * ?");
	Trigger trigger = TriggerBuilder.newTrigger()
    				.withDescription("this is a trigger")
					.withIdentity("trigger1", "Group1").startAt(new Date(time))
					.withSchedule(builder)
					.build();
	
    //注册任务和定时器
    scheduler.scheduleJob(jobDetail, trigger);
    //启动调度器
    scheduler.start();
    log.info("启动时间： "+Instant.now().atZone(ZoneId.of("+8")));
   }
}

```
原文链接：https://blog.csdn.net/u010648555/article/details/54863144
