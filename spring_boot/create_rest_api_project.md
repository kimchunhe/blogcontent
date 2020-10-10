# 使用springboot创建最简单的RestApi项目

1. 先用maven创建基础项目
2. 修改pom文件
``` xml
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.example</groupId>
    <artifactId>spring-rest-api</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
        <relativePath/><!-- lookup parent from repository -->
    </parent>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

3. 在java目录下创建SpringBoot启动文件
``` java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class RestServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(RestServiceApplication.class, args);
    }
}
```

4. 创建RestApi Controller
``` java
import java.util.concurrent.atomic.AtomicLong;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingConroller {
    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @GetMapping("/greeting")
    public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
        return new Greeting(counter.incrementAndGet(), String.format(template, name));
    }
}
```
5. Greeting class
``` java
public class Greeting {
    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}
```
6. maven build
``` bash
mvn clean install -DskipTests
```

7. run rest api
``` bash
java -jar spring-rest-api-1.0-SNAPSHOT.jar
```
8. 在浏览器访问api
```
http://localhost:8080/greeting
```
9. 如果内存不够，增加启动内存大小
``` bash
java -Xms256m -Xmx1024m -jar xxx.jar
java -Xms256m -Xmx1024m -cp xxx.jar xxx.main.Class
```