---
title: "SAST授课-SpringBoot入门" 
date: 2024-11-21T11:30:03+00:00 
# weight: 1
# aliases: ["/first"]
tags: ["SAST"] 
author: "Whitea"
showToc: false
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "2024 SAST.Web第七次授课" 
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/Whitea029/myblog/blob/main/content"
    Text: "Source Code" # edit text
    appendFilePath: true # to append file path to Edit link
---

## 前言

Spring Boot是一个流行的Java框架，用于简化Spring应用程序的创建和部署。它通过提供一个集成的开发环境，使得开发人员能够快速构建高效的应用程序。同时本节课的目的主要在于编码的实践，概念很多，一开始记不住没关系，但一定要跟着写一写！

## 为什么要面向接口编程

面向接口编程（Programming to an Interface）是一种设计理念，在Spring Boot等框架中广泛应用。以下是Spring Boot中面向接口编程的几个重要原因：

- **降低耦合度**：面向接口编程可以减少模块之间的耦合。通过依赖接口而非具体实现，系统的不同部分可以独立开发和修改，降低了修改某一部分时对其他部分的影响。

- **增强可测试性**：使用接口可以方便地进行单元测试。因为可以轻松地创建接口的模拟实现（Mock），在测试中可以用这些模拟对象替代真实对象，从而隔离测试环境，确保测试的独立性。

- **提高灵活性，扩展性和可维护性**：面向接口编程允许在不改变客户端代码的情况下，轻松替换或扩展实现。只需提供新的实现类，而不必修改使用该接口的代码，使得系统更具灵活性。同时当系统的实现细节被封装在接口后，未来的维护和改进工作将更加简单。修改实现不影响接口的定义，减少了维护成本。

- **清晰的设计架构**：接口提供了清晰的契约，使得开发人员能够明确各个模块的功能和责任。这种设计方式也使得团队协作变得更加简单，便于不同开发人员理解系统的整体架构。

### 那跟new一个对象有什么区别呢？

**直接使用具体实现**：

```java
public class EmailService {
    public void sendEmail(String message) {
        // 发送邮件逻辑
    }
}

public class UserService {
    private EmailService emailService = new EmailService();

    public void registerUser(String email) {
        // 注册用户逻辑
        emailService.sendEmail("欢迎注册: " + email);
    }
}
```

**使用接口**：

```java
public interface MessageService {
    void sendMessage(String message);
}

public class EmailService implements MessageService {
    public void sendMessage(String message) {
        // 发送邮件逻辑
    }
}

public class UserService {
    private MessageService messageService;

    public UserService(MessageService messageService) {
        this.messageService = messageService;
    }

    public void registerUser(String email) {
        // 注册用户逻辑
        messageService.sendMessage("欢迎注册: " + email);
    }
}
```

显然通过接口，`UserService` 可以轻松替换 `EmailService` 为其他实现（如 SMS 服务），只需提供不同的实现，而不需修改 `UserService` 的代码，在庞大的项目中，这对维护项目有很大的帮助。

## 三层架构是什么

在Spring Boot中，面向接口编程的最佳实践就是面向接口编程，应用程序通常被组织成三个主要层次：控制层（Controller）、服务层（Service）和数据访问层（Repository/DAO）

- **控制层（Controller）** 控制层是Spring Boot应用程序的入口点，负责处理用户请求并协调其他层次的工作。在Spring Boot中，使用注解（如@RestController等）标识控制器类，处理HTTP请求，并调用服务层方法。控制器的主要目标是接收客户端的请求，并返回响应。它不包含业务逻辑，而是将大部分工作委派给服务层。

- **服务层（Service）** 服务层是应用程序的核心业务逻辑所在的地方。在控制层的协调下，服务层执行所需的操作，例如数据验证、业务规则实施等。服务层的主要目标是封装应用程序的核心功能，并确保应用程序的正确性。在服务层中，通常会使用事务管理来确保数据的完整性和一致性。

- **数据访问层（Repository/DAO）** 数据访问层是负责数据访问操作的层。它通常使用数据库连接来执行数据的增删改查操作。在Spring Boot中，数据访问对象（DAO）通常使用接口实现，以提供更好的抽象和可维护性。通过面向接口编程，可以实现解耦和可替换性，使得在更换底层数据源或更改数据访问方式时更加方便。此外，数据访问层还负责处理与数据持久化相关的操作，例如数据转换、映射等。  
  
  在实际开发中，这三个层次是**协同工作**的。控制层接收用户请求，调用服务层中的业务逻辑方法，并将结果返回给用户。服务层负责执行核心业务逻辑，并使用数据访问层来与数据库进行交互。数据访问层则专注于与数据源进行交互，并向上层提供一致的数据访问接口。
  
  此外，Spring Boot还提供了许多其他的特性来简化开发过程，例如**自动配置**、**starter依赖管理**、**安全性**等。通过这些特性，开发人员可以快速构建出高效、可扩展和易于维护的应用程序。
  
  ```java
  @RestController
  @RequestMapping("/api")
  public class MyController {
      @Autowired
      private MyService myService;
  
      @GetMapping("/users/{id}")
      public ResponseEntity<User> getUserById(@PathVariable Long id) {
          User user = myService.getUserById(id);
          return ResponseEntity.ok(user);
      }
  
      // 其他请求处理方法
  }
  ```
  
  ```java
  @Service
  public class MyService {
      @Autowired
      private UserRepository userRepository;
  
      public User getUserById(Long id) {
          return userRepository.findById(id);
      }
  
      // 其他业务逻辑方法
  }
  ```
  
  ```java
  @Repository
  public interface UserRepository extends JpaRepository<User, Long> {
      User findById(Long id);
  
      // 其他数据库操作方法
  }
  ```

### 一些注解

## Controller层

    private MessageService messageService;
    
    public UserService(MessageService messageService) {
        this.messageService = messageService;
    }

- `@Controller`：`@Controller`是经典的`Controller`层注解，`@Controller`标识的类代表该类是控制器类

- `@RequestMapping`：使用`@RequestMapping`可以对请求进行映射，可以注解在类上或者方法上，在类上的话表示该类所有的方法都是以该地址作为父地址，在方法上就表示可以映射对应的请求到该方法上

- `@GetMapping/@PostMapping`：这两者实际上是`@RequestMapping`对应不同方法的简化版，因为`@RequestMapping`有一个`method`属性，如果该`method`指定为`GET`那么就相当于`@GetMapping`，如果指定为`POST`就相当于`@PostMapping`

- `@ResponseBody`：作用在方法上，将返回的数据进行可能的转换（取决于请求头，转换为`JSON`或`XML`等等，默认的情况下比如单纯字符串就直接返回），比如返回语句为`return "success";`，如果加上了`@ResponseBody`就直接返回`success`，如果不加上就会跳转到`success.jsp`页面

- `@RequestParm`：处理`Contrent-Type`为`application/x-www-form-urlencoded`的内容，可以接受简单属性类型或者对象，支持`GET`+`POST`

- `@RequestBody`：处理`Content-Type`不为`application/x-www-form-urlencoded`的内容（也就是需要指定`Content-Type`），不支持`GET`，只支持`POST`

- `@PathVariable`：可以将占位符的参数传入方法参数，比如`/path/1`，可以将`1`传入方法参数中

- `@PathParm`：与`@RequestParm`一样，一般使用`@RequestParm`

- `@RestController`：相当于`@Controller`+`@ResponseBody`

## Service层

- `@Serice`：是一个增强型的`@Component`，`@Component`表示一个最普通的组件，可以被注入到`Spring`容器进行管理，而`@Service`是专门用于处理业务逻辑的注解，`@Controller`类似，也是一个增强型的`@Component`，专门用于`Controller`层的处理

## Dao层

- `@Repository`：也是一个增强型的`@Component`，注解在持久层中，具有将具体数据库抛出的异常转为`Spring`持久层异常的功能

### 拦截器 & 过滤器

在构建 Web 应用时，我们经常需要对请求进行拦截和处理，以实现诸如身份验证、授权、日志记录等功能。在 Spring Boot 中，为我们提供了两种强大的工具来实现这些功能：过滤器（Filter）和拦截器（Interceptor）

## 拦截器：

是Spring框架特有的组件，主要用于在Controller处理请求的前后进行操作，通常用于权限检查、日志记录、事务管理等，能够更灵活地访问Spring的上下文。

### HandlerInterceptor

常用于对请求进来的url进行拦截

```java
@Component
public class MyInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 前处理
        return true; // 返回true继续请求，false则中断
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        // 后处理
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        // 请求完成后处理
    }
}
```

**我们也需要注册它**

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Resourse
    private MyInterceptor myInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myInterceptor)
                                    .addPathPatterns("/**");
    }
}
```

### ClientHttpRequestInterceptor

常用于对发送出去的请求进行拦截

```java
@Component
public class CustomHeaderInterceptor implements ClientHttpRequestInterceptor {

    @Override
    public ClientHttpResponse intercept(HttpRequest request, byte[] body, ClientHttpRequestExecution execution)
            throws IOException {
        request.getHeaders().set("Custom-Header", "CustomHeaderValue");
        return execution.execute(request, body);
    }
}
```

```java
    class RestClientConfig{

        @Resourse
        private CustomHeaderInterceptor customHeaderInterceptor;

        public RestClient restClient() {
            return RestClient.builder()
                    .baseUrl("http://localhost:8080")
                    .requestInterceptor(customHeaderInterceptor)
                    .build();
        }
    }
```

## 过滤器

在 Spring Boot 中，过滤器（Filter）是用于在 Servlet 容器级别拦截和处理 HTTP 请求的组件。它们通常用于实现诸如身份验证、授权、日志记录、请求和响应的数据转换等功能。过滤器位于整个请求处理链的最前端，因此在请求到达 Spring 应用的任何其他组件之前，都会先经过过滤器处理。

```java
@WebFilter(urlPatterns = "/*")
public class RequestTimingFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) {
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        long startTime = System.currentTimeMillis();
        try {
            chain.doFilter(request, response);
        } finally {
            long endTime = System.currentTimeMillis();
            long duration = endTime - startTime;
            HttpServletRequest httpRequest = (HttpServletRequest) request;
            System.out.println(String.format("%s %s took %d ms", httpRequest.getMethod(), httpRequest.getRequestURI(), duration));
        }
    }

    @Override
    public void destroy() {
    }
}
```

```java
@Configuration
public class FilterConfig {

    @Bean
    public FilterRegistrationBean<RequestTimingFilter> requestTimingFilter() {
        FilterRegistrationBean<RequestTimingFilter> registrationBean = new FilterRegistrationBean<>();
        registrationBean.setFilter(new RequestTimingFilter());
        registrationBean.addUrlPatterns("/*");
        return registrationBean;
    }
}
```

**执行时机**

![](images/get-start-with-spring-boot/001.png)

## JWT & Cookie & Session

Cookie、Session 和 JWT（JSON Web Token）是 Web 应用中常用的技术，用于用户身份验证和状态管理（众所周知，默认情况下，HTTP 协议是无状态的）

### Cookie & Session

**Cookie**（也称为 Web Cookie 或浏览器 Cookie）是服务器发送到用户 Web 浏览器的一小段数据。浏览器可以存储 cookie、创建新 cookie、修改现有 cookie，并将它们与以后的请求一起发送回同一服务器。Cookie 使 Web 应用程序能够存储有限数量的数据并记住状态信息。

Cookie的类型有很多种，这里介绍常用的Session Cookie

**工作流程**

Session Cookie 是特定于服务器的 Cookie，不能传递给生成 Cookie 的计算机以外的任何计算机。服务器创建一个“**会话 ID**”，这是一个随机生成的数字，用于临时存储会话 cookie。此 cookie 存储用户输入等信息，并跟踪用户在网站内的活动。会话 Cookie 中没有存储任何其他信息。

![](images/get-start-with-spring-boot/002.png)

**当你在网上冲浪，是否需要同意Cookie？**

由于会话 cookie **由第一方**（您访问的网站）设置，并且是跟踪您在网站中的导航和记住用户输入所必需的，因此根据 GDPR 它们不需要同意。网站可以在未经同意的情况下在用户的设备上设置会话 Cookie，但应提供有关这些 Cookie 的作用以及为什么需要这些 Cookie 的信息。这通常是通过cookie 同意横幅完成的。

### JWT

JSON Web 令牌 （[JWT](https://jwt.io/introduction)） 是一种开放标准，它定义了一种紧凑且自包含的方式，用于将信息作为 JSON 对象在各方之间安全地传输。此信息是经过数字签名的，因此可以验证和信任。可以使用密钥（使用 **HMAC** 算法）或使用 **RSA** 或 **ECDSA** 的公钥/私钥对对 JWT 进行签名。

尽管 JWT 可以加密以在各方之间提供机密性，但我们将重点介绍*签名*令牌。签名令牌可以验证其中包含的声明的*完整性*，而加密令牌则对其他方*隐藏*这些声明。当使用公钥/私钥对令牌进行签名时，签名还会证明只有持有私钥的一方是签署私钥的一方

在其紧凑形式中，JSON Web 令牌由三个部分组成，由点`(.)`分隔，它们是：

- Header

- Payload

- Signature

belike：` xxxxxxx.yyyyyyyy.zzzzzzzzz`

**Header**：标头*通常由*两部分组成：令牌的类型（JWT）和正在使用的签名算法，例如 HMAC SHA256 或 RSA。然后，此 JSON 经过 **Base64Url** 编码以形成 JWT 的第一部分

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

**Payload**：令牌的第二部分是有效负载，其中包含声明。声明是关于实体（通常是用户）和其他数据的声明。有三种类型的声明：*registered*, *public*, and *private* claims

- **Registered Claims**：这些是一组预定义的声明，不是强制性的，但建议使用，以提供一组有用的、可互操作的声明。**ss** (issuer), **exp** (expiration time), **sub** (subject), **aud** (audience), and [others](https://tools.ietf.org/html/rfc7519#section-4.1)

- **Public Claims**：这些声明可以由使用 JWT 的用户随意定义。但为避免冲突，应在 [IANA JSON Web 令牌注册表](https://www.iana.org/assignments/jwt/jwt.xhtml)中定义它们，或将其定义为包含抗冲突命名空间的 URI。

- **Private Claims**：这些是自定义声明，用于在同意使用它们的各方之间共享信息，既不是*注册*声明，也不是*公开*声明。

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

然后，对有效负载进行 **Base64Url** 编码，以形成 JSON Web 令牌的第二部分。

**Signature**：要创建签名部分，您必须获取编码的标头、编码的有效负载、密钥、标头中指定的算法，并对其进行签名。

```java
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

![](images/get-start-with-spring-boot/003.png)

**项目集成**

```xml
<!-- https://mvnrepository.com/artifact/com.auth0/java-jwt -->
<dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>4.4.0</version>
</dependency>
```

```java
@Slf4j
@Component
public class JwtUtil {

    private final Algorithm algorithm;
    private final JWTVerifier verifier;

    private final Duration defaultExpireTime;

    public JwtUtil(@Value("${app.security.jwt.secret}") String secret,
                   @Value("${app.security.jwt.expiresIn}") Long expiresInMinutes) {
        this.algorithm = Algorithm.HMAC512(secret);
        this.verifier = JWT.require(algorithm).build();
        this.defaultExpireTime = Duration.ofMinutes(expiresInMinutes);
    }

    public DecodedJWT verify(String token) {
        try {
            return verifier.verify(token);
        } catch (JWTVerificationException e) {
            log.error("JWT verification failed: {}", e.getMessage());
            throw new IllegalArgumentException("Invalid JWT token");
        }
    }


    public String generateAccessToken(String id) {
        return generateAccessToken(id, defaultExpireTime);
    }


    public String generateAccessToken(String id, Duration expireTime) {
        Instant now = Instant.now();
        return JWT.create()
                .withSubject(id)
                .withIssuedAt(now)
                .withExpiresAt(now.plus(expireTime))
                .sign(algorithm);
    }


    public String generateAccessTokenWithClaims(String id, Map<String, String> claims) {
        Instant now = Instant.now();
        var jwtBuilder = JWT.create()
                .withSubject(id)
                .withIssuedAt(now)
                .withExpiresAt(now.plus(defaultExpireTime));

        claims.forEach(jwtBuilder::withClaim);

        return jwtBuilder.sign(algorithm);
    }

    public boolean isTokenExpired(DecodedJWT decodedJWT) {
        Instant expiresAt = decodedJWT.getExpiresAt().toInstant();
        return Instant.now().isAfter(expiresAt);
    }
}
```

## Mybatis入门

MyBatis 是一流的持久化框架，支持自定义 SQL、存储过程和高级映射。MyBatis 几乎消除了所有的 JDBC 代码和手动设置参数和检索结果。MyBatis 可以使用简单的 XML 或注解来配置和映射原语、Map 接口和 Java POJO（普通的 Java 对象）到数据库记录。

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>3.0.3</version>
</dependency>
```

用@Mapper注解声明一个mapper作为持久层操作的接口

```java
@Mapper
public interface CityMapper {

  @Select("SELECT * FROM CITY WHERE state = #{state}")
  City findByState(@Param("state") String state);

}
```

随着项目的庞大，大量的sql语句也可以单独写在一个xml文件当中

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="sast.freshcup.mapper.AccountMapper">

    <select id="getStudentsNumberByContestId" resultType="java.lang.Long">
        SELECT COUNT(*)
        FROM account a
                 INNER JOIN account_contest_manager acm ON a.uid = acm.uid
        WHERE acm.contest_id = #{contestId}
          AND a.role=0
          AND a.is_deleted=0
    </select>
</mapper>
```

> 作为一名后端开发,文章存在有任何问题都可以联系我，当然也欢迎与我交流技术相关的问题，感谢你的阅读🤗