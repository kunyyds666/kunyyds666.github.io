# java项目easypan网盘项目

## 1项目配置

### pom.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.1</version>
        <relativePath/>
    </parent>

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.easypan</groupId>
    <artifactId>easypan</artifactId>
    <version>1.0</version>
    <packaging>jar</packaging>
    <name>easypan</name>
    <description>easypan</description>

    <properties>
        <java.version>1.8</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <skipTests>true</skipTests>

        <springboot.version>2.6.1</springboot.version>
        <mybatis.version>1.3.2</mybatis.version>
        <logback.version>1.2.10</logback.version>
        <mysql.version>8.0.23</mysql.version>
        <aspectjweaver.version>1.9.4</aspectjweaver.version>
        <fastjson.version>1.2.66</fastjson.version>
        <commons.lang3.version>3.4</commons.lang3.version>
        <commons.codec.version>1.9</commons.codec.version>
        <commons.io.version>2.5</commons.io.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>ch.qos.logback</groupId>
                    <artifactId>logback-classic</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>ch.qos.logback</groupId>
                    <artifactId>logback-core</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <!--邮件发送-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
            <version>${springboot.version}</version>
        </dependency>
        <!--redis -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
            <version>${springboot.version}</version>
        </dependency>

        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>${mybatis.version}</version>
        </dependency>


        <!-- 数据库-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql.version}</version>
        </dependency>


        <!-- 日志版本 -->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>${logback.version}</version>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
            <version>${logback.version}</version>
        </dependency>

        <!--切面-->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>${aspectjweaver.version}</version>
        </dependency>
        <!--fastjson-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>${fastjson.version}</version>
        </dependency>

        <!--apache common-->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>${commons.lang3.version}</version>
        </dependency>

        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>${commons.codec.version}</version>
        </dependency>

        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>${commons.io.version}</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.2.6.RELEASE</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>
                                repackage
                            </goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <mainClass>com.easypan.EasyPanApplication</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```

### application.properties配置

```properties
# 应用服务 WEB 访问端口
server.port=7090
server.servlet.context-path=/api
#session过期时间 60M 一个小时
server.servlet.session.timeout=PT60M
#处理favicon
spring.mvc.favicon.enable=false
#异常处理
spring.mvc.throw-exception-if-no-handler-found=true
spring.web.resources.add-mappings=false
#数据库配置
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/easypan?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&autoReconnect=true&allowMultiQueries=true
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.hikari.pool-name=HikariCPDatasource
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.idle-timeout=180000
spring.datasource.hikari.maximum-pool-size=10
spring.datasource.hikari.auto-commit=true
spring.datasource.hikari.max-lifetime=1800000
spring.datasource.hikari.connection-timeout=30000
spring.datasource.hikari.connection-test-query=SELECT 1
#发送邮件配置相关
# 配置邮件服务器的地址 smtp.qq.com
spring.mail.host=smtp.qq.com
# 配置邮件服务器的端口（465或587）
spring.mail.port=465
# 配置用户的账号
spring.mail.username=test@qq.com
# 配置用户的密码
spring.mail.password=123456
# 配置默认编码
spring.mail.default-encoding=UTF-8
# SSL 连接配置
spring.mail.properties.mail.smtp.socketFactory.class=javax.net.ssl.SSLSocketFactory
# 开启 debug，这样方便开发者查看邮件发送日志
spring.mail.properties.mail.debug=true
#邮件配置结束
#Spring redis配置
# Redis数据库索引（默认为0）
spring.data.redis.database=0
spring.data.redis.host=localhost
spring.data.redis.port=6379
# 连接池最大连接数（使用负值表示没有限制）
spring.data.redis.jedis.pool.max-active=20
# 连接池最大阻塞等待时间（使用负值表示没有限制）
spring.data.redis.jedis.pool.max-wait=-1
# 连接池中的最大空闲连接
spring.data.redis.jedis.pool.max-idle=10
# 连接池中的最小空闲连接
spring.data.redis.jedis.pool.min-idle=0
# 连接超时时间（毫秒）
spring.data.redis.timeout=2000
#项目目录
#project.folder=X:/code/MavenProjects/easypan/
#日志级别配置
logging.level.root=DEBUG
#超级管理员id
#admin.emails=test@qq.com
#是否是开发环境
#dev=false
##qq登陆相关##
#qq.app.id=12333
#qq.app.key=2222222
#qq.url.authorization=https://graph.qq.com/oauth2.0/authorize?response_type=code&client_id=%s&redirect_uri=%s&state=%s
#qq.url.access.token=https://graph.qq.com/oauth2.0/token?grant_type=authorization_code&client_id=%s&client_secret=%s&code=%s&redirect_uri=%s
#qq.url.openid=https://graph.qq.com/oauth2.0/me?access_token=%S
#qq.url.user.info=https://graph.qq.com/user/get_user_info?access_token=%s&oauth_consumer_key=%s&openid=%s
#qq.url.redirect=http://easypan.wuhancoder.com/qqlogincalback

```



### logback-spring.xml配置

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration scan="true" scanPeriod="10 minutes">
    <appender name="stdot" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>%d{yyyy-MM-dd HH:mm:ss,GMT+8} [%p][%c][%M][%L]-> %m%n</pattern>
        </layout>
    </appender>

    <springProperty scope="context" name="log.path" source="project.folder"/>
    <springProperty scope="context" name="log.root.level" source="log.root.level"/>

    <property name="LOG_FOLDER" value="logs"/>
    <property name="LOG_FILE_NAME" value="easypan.log"/>

    <appender name="file" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${log.path}/${LOG_FOLDER}/${LOG_FILE_NAME}</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <FileNamePattern>${log.path}/${LOG_FOLDER}/${LOG_FILE_NAME}.%d{yyyyMMdd}.%i</FileNamePattern>
            <cleanHistoryOnStart>true</cleanHistoryOnStart>
            <TimeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <MaxFileSize>20MB</MaxFileSize>
            </TimeBasedFileNamingAndTriggeringPolicy>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <encoder>
            <charset>utf-8</charset>
            <pattern>%d{yyyy-MM-dd HH:mm:ss,GMT+8} [%p][%c][%M][%L]-> %m%n</pattern>
        </encoder>
        <append>false</append>
        <prudent>false</prudent>
    </appender>

    <root level="${log.root.level}">
        <appender-ref ref="stdot"/>
        <appender-ref ref="file"/>
    </root>

</configuration>

```

## 2启动测试类

easypanApplication

```java
package com.ks;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@EnableAsync//异步调用
@SpringBootApplication(scanBasePackages = "com.ks")
@EnableTransactionManagement//启动事务
@EnableScheduling
public class easypanApplication {
    public static void main(String[] args) {
        SpringApplication.run(easypanApplication.class, args);
    }
}

```

TestController

```java
package com.ks.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class TestController {


    @RequestMapping("/test")
    public String test(){
        return "test";
    }


}
```

配置热部署Jrebel插件

![image-20240714233044018](X:\md\Project\easypan\easy\image-20240714233044018.png)

开始破解

下载破解工具运行

https://github.com/ilanyu/ReverseProxy/releases/download/v1.4/ReverseProxy_windows_386.exe

生成GUID

[Create GUID online (guidgen.com)](https://www.guidgen.com/)

激活Jrebel

TeamUrl  http://127.0.0.1:8888/{GUID}

邮箱随意

设置为离线

勾选项目,shift+F9热部署

<img src="X:\md\Project\easypan\easy\image-20240714234344975.png" alt="image-20240714234344975"  />































