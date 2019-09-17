hana studio 连接

一

```java
Cf login https://api.cf.sap.hana.ondemand.com
User-ID/PASSWORD
cf ssh workforce-person-srv -L localhost:30015:zeus.hana.canary.eu-central-1.whitney.dbaas.ondemand.com:23869
```

`workforce-person-srv` appplication name

localhost:30015，`30015` 是自己定义的。后面的`zeus.hana.canary.eu-central-1.whitney.dbaas.ondemand.com:23869` 去cloud foundry 那里找**host : port**

- Setup ssh connection via **"cf ssh  -L localhost:30041:zeus.hana.canary.eu-central-1.whitney.dbaas.ondemand.com:23869"**, change **zeus.hana.canary.eu-central-1.whitney.dbaas.ondemand.com:23869** to your own hana instance.
- Bash prompt like following shows the tunnel is successful, and keep the bash window(tunnel) open. **vcap@4c34af53-b679-4500-7e2b-d348:~$**





二

1. 打开hana studio
2. 在System面板中空白区域鼠标右键菜单中选择【add System】
3. 弹出的窗口中填写，其中Host Name中不用添加端口号，可自动识别(e.x. **localhost**)。Instance Number设置（通常是00或者1）
4. 弹出【Connection Properties】窗口中，输入用户名和密码（cloud foundry 提供），勾选**connect using ssl**点击底部的Next. 
5. 进入【Additional Properties】, override Host Name in certificate 中输入 **host**（e.x. **zeus.hana.canary.eu-central-1.whitney.dbaas.ondemand.com**）



### Karate

基于BDD(Behavior Driven Development)自动化测试框架 —cocumber

`Results`   check if any scenarios failed, and to even summarize the errors



#### JUnit 4 Parallel Execution

```java
import com.intuit.karate.KarateOptions;
import com.intuit.karate.Results;
import com.intuit.karate.Runner;
import static org.junit.Assert.*;
import org.junit.Test;

@KarateOptions(tags = {"~@ignore"})
public class TestParallel {
    
    @Test
    public void testParallel() {
        Results results = Runner.parallel(getClass(), 5, "target/surefire-reports");
        assertTrue(results.getErrorMessages(), results.getFailCount() == 0);
    }
    
}
```

#### Script Structure

##### Feature

#####  Backgroud

- this section is optional
- steps here are executed before each Scenario in this file
- variables defined here will be global to all scnarios
- and will be re-initialized befror every scenario

#####  Scenario

变体 **Scenario Outline**



#### Parallel Execution

**@parallel=false**

阻止默认parallel执行的Scenario

放置在关键字`Feature` 之前，将适用于全部的`Scenario`

也可以选择一到两个`Scenario`之前,阻止其并发执行。

#### Karate Options

- 指定一个feature文件执行 利用annotion `@KarateOptions`

```java
package animals.cats;

import com.intuit.karate.KarateOptions;
import com.intuit.karate.junit4.Karate;
import org.junit.runner.RunWith;

@RunWith(Karate.class)
@KarateOptions(features = "classpath:animals/cats/cats-post.feature")
public class CatsPostRunner {
	
}
```

- annotation 中的参数`features`也可以是一个数组

```java
@KarateOptions(features = {
    "classpath:animals/cats/cats-post.feature",
    "classpath:animals/cats/cats-get.feature"})
```

- 也可以指定a directory (or package)，结合`tags`执行多个features，不需要都列出它们。

```java
@KarateOptions(features="classpath:animals/cats",tags="~@ingore")
//this will run all feature files in 'animals/cats'
//except the ones marked as @ingore
```

#### Logging

Karate 使用LOGBack 在`classpath`中查找一个文件叫做`logback-test.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>
  
    <appender name="FILE" class="ch.qos.logback.core.FileAppender">
        <file>target/karate.log</file>
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>    
   
    <logger name="com.intuit.karate" level="DEBUG"/>
   
    <root level="info">
        <appender-ref ref="STDOUT" />
        <appender-ref ref="FILE" />
    </root>
  
</configuration>
```

 

```xml
   <!--
        Appender: 设置日志信息的去向,常用的有以下几个
            ch.qos.logback.core.ConsoleAppender (写到控制台)
            ch.qos.logback.core.rolling.RollingFileAppender (文件大小到达指定尺寸的时候产生一个           新文件)
            ch.qos.logback.core.FileAppender (写到文件)
    -->

    <!--
        用来设置某一个包或者具体的某一个类的日志打印级别、以及指定<appender>。
        <logger>仅有一个name属性，一个可选的level和一个可选的addtivity属性
        name:
            用来指定受此logger约束的某一个包或者具体的某一个类。
        level:
            用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF，
            如果未设置此属性，那么当前logger将会继承上级的级别。
        additivity:
            是否向上级loger传递打印信息。默认是true。
        <logger>可以包含零个或多个<appender-ref>元素，标识这个appender将会添加到这个logger
    -->
    
    <!--
        root也是<logger>元素，但是它是根logger。默认debug
        level:用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF，
        <root>可以包含零个或多个<appender-ref>元素，标识这个appender将会添加到这个logger。
    -->
```

####  Configuration

start-up Karate  `karate-config.js`  在`classpath`

### Keywords



#### Headers

可以在`Background：` 中设置如下，将适用于此feature中的所有Scenario

```java
* configure headers = { 'Content-Type': 'application/xml' }
```

#### call

#### CallOnce

when you use the combination of  `callonece` In a  `backgroud`, you can indeed get the same effect as using a `@BeforeClass` annotation

####  Run test classes

l`test\KarateDSL`  

执行test

```shell
mvn clean install -U -DTEST_ROUTE=https://workforcebusinessservicesh-trainingwbs-wenxue-workforce4672e1e9.cfapps.sap.hana.ondemand.com/ -Dtest=WBSRunner 
```

mvn clean install -U -DTEST_ROUTE= http://localhost:8080/WorkforcePersonServiceOData_srv_war_exploded -Dtest=DPPRunner 

 

 ```java
https://workforcebusinessservicesh-trainingwbs-wenxue-workforce4672e1e9.cfapps.sap.hana.ondemand.com/odata/v4/WorkforceAPI/WorkforcePersons?$filter=externalID eq '1111'
 ```



```java

    And match response.value[*].ID contains [03ca617c-730b-47a1-9236-5fa5928ab4ac]
```











