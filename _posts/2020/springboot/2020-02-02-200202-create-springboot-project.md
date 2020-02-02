---
title: "Maven으로 Spring Boot 프로젝트 생성하기"
excerpt: "Maven 으로 Spring Boot 프로젝트 생성한다."
search: true
categories:
  - Spring Boot
tags:
  - Spring Boot
  - Maven
last_modified_at: 2020-02-02T17:09:00+09:00
toc: true
toc_sticky: true
comments: true
# header:
#   teaser:
# install-docker:
#   - url:
#     image_path:
#     alt: ""
#     title: ""
---

# 0. 실행 환경

- OS: `MAC OS`
- 준비물: `java`, `Maven`

# 1. Maven 프로젝트 생성

아래 명령어를 통해 메이븐 프로젝트를 생성한다.

```
$ mvn archetype:generate
```

```
... 생략 ...

Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): 1471:
Choose org.apache.maven.archetypes:maven-archetype-quickstart version:
1: 1.0-alpha-1
2: 1.0-alpha-2
3: 1.0-alpha-3
4: 1.0-alpha-4
5: 1.0
6: 1.1
7: 1.3
8: 1.4
Choose a number: 8: 8

... 생략 ...

Define value for property 'groupId': com.example
Define value for property 'artifactId': test_project
Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
Define value for property 'package' com.example: : test
Confirm properties configuration:
groupId: com.example
artifactId: test_project
version: 0.0.1-SNAPSHOT
package: test
 Y: : y
```

# 2. 코드 작성

메이븐은 `pom.xml` 를 이용하여 필요한 아티팩트를 정의하거나 기타 정보를 설정한다.

## pom.xml 작성

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.example</groupId>
  <artifactId>test_project</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <name>test_project</name>
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <!-- Spring Boot -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.4.RELEASE</version>
  </parent>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
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
</project>

```

## java 코드 생성

파일 이름은 `TestApp.java` 로 했지만 이름은 중요하지 않다. (원하는 이름으로 하면 됨)

```java
package test;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@EnableAutoConfiguration
public class TestApp
{
    @RequestMapping("/")
    public String root() {
        return "Root Page";
    }

    public static void main( String[] args ) {
        SpringApplication.run(TestApp.class, args);
    }
}
```

# 2. 프로젝트 빌드 및 실행

생성된 `spring boot` 앱을 실행해본다.

```
$ mvn spring-boot:run

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.2.4.RELEASE)

2020-02-02 16:52:03.703  INFO 34696 --- [           main] test.TestApp                             : Starting TestApp on TaehongKimui-MacBookPro.local with PID 34696 (/Users/taehongkim/dev/spring/test_project/target/classes started by taehongkim in /Users/taehongkim/dev/spring/test_project)

... 생략 ...
```

브라우저를 통해 `localhost:8080` 으로 접속한다.

화면에 `Root Page` 라는 말이 나오면, 제대로 스프링부트 프로젝트 생성된 것.

**Maven 빌드하여 실행하기**

또는 메이븐으로 빌드하여 `.jar` 파일을 실행하는 방법도 있다.

```
$ mvn clean package
$ java -jar target/test_project-0.0.1-SNAPSHOT.jar
```

결과는 동일하다.

# Reference

- <a href="https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#getting-started" target="_blank">https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#getting-started</a>
