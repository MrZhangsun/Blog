---
title: 利用JDK和spring-task实现高可用定时任务
date: 2018-01-18 11:11:59
tags: Schedule
---
### 1. 定时任务框架简介：
当前定时任务的实现方式主要有： 采用Quartz、Spring-Task、JDK、Zookeeper、Reids、Elastic-Job等。本文只介绍Spring-Task和JDK定时任务的使用。 主要是因为对于Java开发来说，这两个定时任务是不需要引入任何外部Jar包的，属于轻量级定时任务框架。当然，该文适合简单的定时任务，对于复杂的定时任务可采用定时任务调度服务器的方式进行实现。本文将以SpringBoot为框架，以动态维护活动开始和结束时间为例，介绍Spring-Task和JDK定时任务的实现。
### 2. Spring-Task的使用：
- App.java类中添加如下注解：
		
		
		@EnableAsync  
		@SpringBootApplication  
		@ComponentScan  
		@EnableAutoConfiguration  
		@EnableScheduling  
		public class App extends SpringBootServletInitializer {  
		    public static void main(String[] args) {  
		        SpringApplication.run(App.class, args);  
		    }  
		}  
- 编写任务类ActivityTask.java

	
		package com.bldz.promotion.task;  
		
		import com.bldz.promotion.common.Constants;  
		import com.bldz.promotion.service.ActivityService;  
		import org.slf4j.Logger;  
		import org.slf4j.LoggerFactory;  
		import org.springframework.beans.factory.annotation.Autowired;  
		import org.springframework.scheduling.annotation.Scheduled;  
		import org.springframework.stereotype.Component;  
		
		/**
		 * 更新活动时间  
		 *
		 * @author ZhangsunJiankun 2018年1月11日18:06:05  
		 */
		@Component  
		public class ActivityTask {  
		
		    private final ActivityService activityService;  
		
		    private static Logger logger = LoggerFactory.getLogger  (ActivityTask.class);  
		
		    @Autowired  
		    public ActivityTask(ActivityService activityService) {  
		        this.activityService = activityService;  
		    }  
		
		    /**
		     * 更新活动开始时间  
		     */
		    @Scheduled(cron = "0 0/5 * * * ?")  
		    public void updateToHaving() {  
		        this.update(Constants.ACTIVITY_STATUS_HAVING);  
		    }  
		
		    /**
		     * 更新活动时间为已结束  
		     */
		    @Scheduled(cron = "0 0/5 * * * ?")  
		    public void updateToEnd() {  
		        this.update(Constants.ACTIVITY_STATUS_OVER);  
		    }  
		
		    private void update(String status) {  
		        if(Constants.ACTIVITY_STATUS_HAVING.equals(status)) {  
		            int items = activityService.updateStatusToHaving();  
		            if(items > 0) {  
		                logger.info("update activity status to having successfully!   Number of updates:{} ", items);  
		            }  
		        } else if(Constants.ACTIVITY_STATUS_OVER.equals(status)){  
		            int items = activityService.updateStatusToEnd();  
		            if(items > 0) {  
		                logger.info("update activity status to end successfully!   Number of updates:{} ", items);  
		            }  
		        } else {  
		            logger.error("update status error! status:{}", status);  
		        }  
		    }  
		}  

###### 至此Spring-Task定时任务已经完成，是不是很简单:） 难点主要在于@Schedule注解的使用，接下来我就说说这个注解中的参数含义，及使用。
		@Schedule(String corn, Long fixedDelay, Long fixedRate, Long initialDelay, Long zone)
		corn: 定时任务表达式
		fixedDelay: 以固定的频率，在最后一次调用的完成之后到下一此调用开始之间执行该方法 (以毫秒为单位)。
		initialDelay: Spring容器启动之后第一次开始执行该Job的推迟时间
		fixedRate: 以固定的频率执行该方法
		zone: 服务器所在的时区
###### corn表达式的使用
		Cron表达式是一个字符串，字符串以5或6个空格隔开，分为6或7个域（Spring-Task只支持6个域），每一个域代表一个含义，每一个域可出现的字符如下： 
		Seconds:可出现", - * /"四个字符，有效范围为0-59的整数 
		Minutes:可出现", - * /"四个字符，有效范围为0-59的整数 
		Hours:可出现", - * /"四个字符，有效范围为0-23的整数 
		DayofMonth:可出现", - * / ? L W C"八个字符，有效范围为0-31的整数 
		Month:可出现", - * /"四个字符，有效范围为1-12的整数或JAN-DEc 
		DayofWeek:可出现", - * / ? L C #"四个字符，有效范围为1-7的整数或SUN-SAT两个范围。1表示星期天，2表示星期一， 依次类推 
		Year:可出现", - * /"四个字符，有效范围为1970-2099年
		
		每一个域都使用数字，但还可以出现如下特殊字符，它们的含义是： 
		(1)*：表示匹配该域的任意值，假如在Minutes域使用*, 即表示每分钟都会触发事件。
		(2)?:只能用在DayofMonth和DayofWeek两个域。它也匹配域的任意值，但实际不会。因为DayofMonth和DayofWeek会相互影响。例如想在每月的20日触发调度，不管20日到底是星期几，则只能使用如下写法： 13 13 15 20 * ?, 其中最后一位只能用？，而不能使用*，如果使用*表示不管星期几都会触发，实际上并不是这样。 
		(3)-:表示范围，例如在Minutes域使用5-20，表示从5分到20分钟每分钟触发一次 
		(4)/：表示起始时间开始触发，然后每隔固定时间触发一次，例如在Minutes域使用5/20,则意味着5分钟触发一次，而25，45等分别触发一次. 
		(5),:表示列出枚举值值。例如：在Minutes域使用5,20，则意味着在5和20分每分钟触发一次。 
		(6)L:表示最后，只能出现在DayofWeek和DayofMonth域，如果在DayofWeek域使用5L,意味着在最后的一个星期四触发。 
		(7)W:表示有效工作日(周一到周五),只能出现在DayofMonth域，系统将在离指定日期的最近的有效工作日触发事件。例如：在 DayofMonth使用5W，如果5日是星期六，则将在最近的工作日：星期五，即4日触发。如果5日是星期天，则在6日(周一)触发；如果5日在星期一到星期五中的一天，则就在5日触发。另外一点，W的最近寻找不会跨过月份 
		(8)LW:这两个字符可以连用，表示在某个月最后一个工作日，即最后一个星期五。 
		(9)#:用于确定每个月第几个星期几，只能出现在DayofMonth域。例如在4#2，表示某月的第二个星期三。
		
		举几个例子: 
		0 0 2 1 * ? * 表示在每月的1日的凌晨2点调度任务 
		0 15 10 ? * MON-FRI 表示周一到周五每天上午10：15执行作业 
		0 15 10 ? 6L 2002-2006 表示2002-2006年的每个月的最后一个星期五上午10:15执行作
		0 0 10,14,16 * * ? 每天上午10点，下午2点，4点 
		0 0/30 9-17 * * ? 朝九晚五工作时间内每半小时 
		0 0 12 ? * WED 表示每个星期三中午12点 
		"0 0 12 * * ?" 每天中午12点触发 
		"0 15 10 ? * *" 每天上午10:15触发 
		"0 15 10 * * ?" 每天上午10:15触发 
		"0 15 10 * * ? *" 每天上午10:15触发 
		"0 15 10 * * ? 2005" 2005年的每天上午10:15触发 
		"0 * 14 * * ?" 在每天下午2点到下午2:59期间的每1分钟触发 
		"0 0/5 14 * * ?" 在每天下午2点到下午2:55期间的每5分钟触发 
		"0 0/5 14,18 * * ?" 在每天下午2点到2:55期间和下午6点到6:55期间的每5分钟触发 
		"0 0-5 14 * * ?" 在每天下午2点到下午2:05期间的每1分钟触发 
		"0 10,44 14 ? 3 WED" 每年三月的星期三的下午2:10和2:44触发 
		"0 15 10 ? * MON-FRI" 周一至周五的上午10:15触发 
		"0 15 10 15 * ?" 每月15日上午10:15触发 
		"0 15 10 L * ?" 每月最后一日的上午10:15触发 
		"0 15 10 ? * 6L" 每月的最后一个星期五上午10:15触发 
		"0 15 10 ? * 6L 2002-2005" 2002年至2005年的每月的最后一个星期五上午10:15触发 
		"0 15 10 ? * 6#3" 每月的第三个星期五上午10:15触发

### 3. JDK定时时任务的实现：

- 编写TaskUtil.java

	
		package com.bldz.promotion.common.utils;
		
		import com.google.common.util.concurrent.ThreadFactoryBuilder;
		import org.apache.commons.lang3.concurrent.BasicThreadFactory;
		import java.util.concurrent.*;
		
		/**
		 * Description: 基于JDK的定时任务工具类
		 *
		 * @author Jiankun.Zhangsun 2018/1/11 17:20
		 */
		public class TaskUtils {
		
		    private static ThreadFactory threadFactory = new ThreadFactoryBuilder().setNameFormat("demo-pool-%d").build();
		
		    public static void schedule(Runnable task, Long delayed, TimeUnit timeUnit) {
		        ScheduledExecutorService scheduleService = Executors.newScheduledThreadPool(5, threadFactory);
		        scheduleService.schedule(task, delayed, timeUnit);
		        scheduleService.shutdown();
		    }
		}
		


- 编写StatusTask.java


		package com.bldz.promotion.task;
		
		import com.bldz.promotion.exceptions.PromotionException;
		import com.bldz.promotion.service.ActivityService;
		import org.slf4j.Logger;
		import org.slf4j.LoggerFactory;
		
		/**
		 * Description:
		 *
		 * @author Jiankun.Zhangsun 2018/1/16 15:43
		 */
		public class StatusTask implements Runnable {
		
		    private Logger logger = LoggerFactory.getLogger(StatusTask.class);
		
		    private ActivityService activityService;
		
		    private String status;
		
		    private String key;
		
		    public StatusTask(String status, String key, ActivityService activityService) {
		        this.status = status;
		        this.key = key;
		        this.activityService = activityService;
		    }
		
		    @Override
		    public void run() {
		        int update;
		        // 新线程中的异常不会在main线程中抛出来
		        try {
		            update = activityService.updateActivityStatus(key, status);
		        } catch (PromotionException e) {
		            logger.error("update activity status to " + status + " failed!! failed reason:{}", e) ;
		            return;
		        }
		        if(update > 0) {
		            logger.info("update activity status to " + status + " successfully!!");
		        } else {
		            logger.error("update activity status to " + status + " failed!! update key:{} failed reason:{}", key, "database error!") ;
		        }
		    }
		}


### 4. 总结

Spring-Task和JDK都是JavaEE开发中常用到的，不需要额外引入，相对轻量级。 两者分别存在以下优缺点：
1. @Schedule中所有参数都是常量，不支持变量，必须在容器启动的时候就开始执行；
2. JDK中的定时任务，是通过重启一个线程去执行定时任务的，这个线程根据需求随时可以启动，其中的delay参数支持变量的方式；
3. JDK中的定时任务如果宕机了，或者服务器重启了会停止执行，所以采用Spring-Task每5分钟执行一次进行保证定时任务的高可用。
