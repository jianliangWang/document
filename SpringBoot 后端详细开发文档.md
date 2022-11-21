# SpringBoot 后端详细开发文档

## 技术栈

1. SpringBoot

2. mybatis plus

3. Spring Security

4. JWT

5. Hutool

6. Redis

7. MySQL

8. SpringTest

9. Maven

## 开发工具

- intelliJ IDEA
- Navicat for MySQL

## 框架搭建

### 1.创建一个SpringBoot项目

![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-30-09-55-25-image.png)

![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-30-09-56-11-image.png)

### 2. 集成mybatis plus

在pom文件中添加依赖

```xml
    <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter</artifactId>
      <version>${mybatis-plus.version}</version>
    </dependency>
```

增加数据库mysql依赖

```xml
 <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.23</version>
      <scope>runtime</scope>
    </dependency>
```

配置数据库连接

```yml
server:
  port: 8888

spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/system

    username: root
    password: 123456
mybatis-plus:
  mapper-locations: classpath*:/mapper/**Mapper.xml
```

### 3. 集成Swagger

- swagger官网 
  
  https://swagger.io/

- 我们使用的是spring，所以直接应该使用spring集成好swagger的springfox，官网地址：https://github.com/springfox/springfox

- 添加依赖
  
  - maven
    
    ```xml
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-boot-starter</artifactId>
        <version>3.0.0</version>
    </dependency>
    ```
  
  - gradle
    
    ```groovy
    implementation "io.springfox:springfox-boot-starter:<version>"
    ```

- 最简单的配置
  
  - 创建配置类
    
    ```java
    package com.wjl.system.config;
    
    import org.springframework.context.annotation.Configuration;
    import springfox.documentation.swagger2.annotations.EnableSwagger2;
    
    @Configuration
    @EnableSwagger2
    public class SwaggerConfig {
    
    }
    ```
  
  - 在controller上添加注解
    
    ```java
    @Api(tags = "登录")
    @RestController
    public class LoginController {
    
        @Operation(summary = "获取验证码")
        @GetMapping("/captcha")
        public ResultJson captcha() throws IOException {
            return ResultJson.success();
    
        }
    }
    ```
  
  - 配置文件配置，兼容swagger3.0
    
    ```yml
    spring:
      mvc:
        pathmatch:
          matching-strategy: ANT_PATH_MATCHER
    ```
  
  - 访问：
    
    http://localhost:8888/swagger-ui/index.html





### 4. 创建表

- system_user

- system_role

- system_authorization

- system_user_role

- system_role_authorization

### 5. 代码生成

1. 添加依赖
   
   ```xml
   <!--生成代码所需要的包-->
       <dependency>
         <groupId>com.baomidou</groupId>
         <artifactId>mybatis-plus-generator</artifactId>
         <version>3.5.2</version>
         <scope>compile</scope>
       </dependency>
   
       <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-freemarker</artifactId>
         <version>2.6.5</version>
         <scope>compile</scope>
       </dependency>
   ```

2. 配置生成代码类
   
   ```java
   package com.wjl.system.generator;
   
   import com.baomidou.mybatisplus.generator.FastAutoGenerator;
   import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
   import com.baomidou.mybatisplus.generator.config.OutputFile;
   import com.baomidou.mybatisplus.generator.config.querys.MySqlQuery;
   import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;
   import java.util.Collections;
   
   public class GeneratorCode {
   
       public static void main(String[] args) {
           FastAutoGenerator.create(
                   // 数据库配置
                   new DataSourceConfig.Builder("jdbc:mysql://localhost:3306/system", "root", "123456")
                       .dbQuery(new MySqlQuery()))
               // 全局配置
               .globalConfig(builder -> {
                   builder.author("jay") // 设置作者
                       .outputDir("/Users/jay/pojo"); // 指定输出目录
               })
               .packageConfig(builder -> {
                   builder.parent("com.wjl") // 设置父包名
                       .moduleName("system") // 设置父包模块名
                       .entity("entity")
                       .service("service")
                       .serviceImpl("service.impl")
                       .mapper("mapper")
                       .pathInfo(Collections.singletonMap(OutputFile.xml, "/Users/jay/pojo")); // 设置mapperXml生成路径
               })
               .strategyConfig(builder -> {
                   builder.addInclude("system_user")
                       .controllerBuilder()
                       .enableRestStyle();
               })
               .templateEngine(new FreemarkerTemplateEngine()) // 使用Freemarker引擎模板，默认的是Velocity引擎模板
               .execute();
       }
   }
   ```

3. 执行生成类
   
   生成代码省略...

### 6.单元测试

引入测试依赖

```xml
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
    
     <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
      <scope>test</scope>
    </dependency>
```

## 项目最终的pom文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.6.5</version>
  </parent>
  <groupId>com.wjl</groupId>
  <artifactId>system</artifactId>
  <version>1.0-SNAPSHOT</version>
  <name>system</name>
  <description>system</description>
  <properties>
    <java.version>17</java.version>
    <mybatis-plus.version>3.5.1</mybatis-plus.version>
    <mysql.version>8.0.23</mysql.version>
    <mybatis-plus-generator>3.5.2</mybatis-plus-generator>
    <springboot-starter-freemarker.version>2.6.5</springboot-starter-freemarker.version>
  </properties>
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter</artifactId>
      <version>${mybatis-plus.version}</version>
    </dependency>

    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>${mysql.version}</version>
      <scope>runtime</scope>
    </dependency>

    <!--生成代码所需要的包-->
    <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-generator</artifactId>
      <version>${mybatis-plus-generator}</version>
      <scope>compile</scope>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-freemarker</artifactId>
      <version>${springboot-starter-freemarker.version}</version>
      <scope>compile</scope>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
      <scope>test</scope>
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

## 结果Result类的封装

> 因为是前后端分离项目，所以我们有必要统一一个返回结果，这样前后端交互的时候有一个统一的标准，约定结果返回的数据是正常的或者遇到异常了。

> 我们这里封装了一个Result类，这个用于我们的异步统一返回结果封装。一般来啊说结果里面有几个必要要素
> 
> - 是否成功，用code表示（200表示成功，400表示异常等）
> 
> - 返回消息，用msg表示（成功，失败）
> 
> - 结果数据，用data表示

封装类：

```java
package com.wjl.system.common;

import java.io.Serializable;

public class Result implements Serializable {

    private int code;

    private String msg;

    private Object data;

    public static Result success (Object data) {
        return success("操作成功", data);
    }

    public static Result success (String msg, Object data) {
        return result(200, msg, data);
    }

    public static Result fail(int code, String msg){
        return result(code, msg, null);
    }

    public static Result fail(String msg){
        return fail(400, msg);
    }
    public static Result result (int code, String msg, Object data) {
        Result result = new Result();
        result.setCode(code);
        result.setMsg(msg);
        result.setData(data);
        return result;
    }

    public int getCode() {
        return code;
    }

    public void setCode(int code) {
        this.code = code;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    public Object getData() {
        return data;
    }

    public void setData(Object data) {
        this.data = data;
    }
}
```

## 全局异常处理

此处异常随着义务的增加而不断完善，这里是最基本的

```java
@RestControllerAdvice
public class GlobalException {

    Logger logger = LoggerFactory.getLogger(GlobalException.class);

    @ResponseStatus(HttpStatus.BAD_REQUEST)
    @ExceptionHandler
    public Result handler(IllegalArgumentException e) {
        logger.error("Assert异常：---------{}", e.getMessage());
        return Result.fail(e.getMessage());
    }

    @ResponseStatus(HttpStatus.BAD_REQUEST)
    @ExceptionHandler
    public Result handler(RuntimeException e){
        logger.error("运行时异常：---------{}", e.getMessage());
        return Result.fail(e.getMessage());
    }
}
```

## 整合Spring Security

登录后端流程图

![](/Users/jay/Library/Application%20Support/marktext/images/2022-03-30-16-55-09-image.png)

### 前期准备工作

#### 引入Spring Sercurity

1. 引入依赖
   
   ```xml
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-security</artifactId>
      </dependency>
   ```

#### 整合Redis

- 引入Redis依赖
  
  ```xml
   <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
      </dependency>
  ```

- 配置文件配置Redis连接
  
  ```yml
  spring:
       redis:
          host: localhost
          password:
          port: 6379
  ```
  
  这样就可以连接redis了，为了方便我们还增加了一个redis的工具类

- RedisUtil
  
  ```java
  package com.wjl.system.utils;
  
  import java.util.Arrays;
  import java.util.List;
  import java.util.Map;
  import java.util.Set;
  import java.util.concurrent.TimeUnit;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.data.redis.core.RedisTemplate;
  import org.springframework.stereotype.Component;
  
  @Component
  public class RedisUtil {
  
      @Autowired
      private RedisTemplate<String, Object> redisTemplate;
  
      public void setRedisTemplate(RedisTemplate<String, Object> redisTemplate) {
          this.redisTemplate = redisTemplate;
      }
  
      /**
       * 指定缓存失效时间
       * @param key 键
       * @param time 时间(秒)
       * @return 是否成功
       */
      public boolean expire(String key, long time) {
          try {
              if (time > 0) {
                  redisTemplate.expire(key, time, TimeUnit.SECONDS);
              }
              return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * 根据key 获取过期时间
       * @param key 键 不能为null
       * @return 时间(秒) 返回0代表为永久有效
       */
      public long getExpire(String key) {
          return redisTemplate.getExpire(key, TimeUnit.SECONDS);
      }
  
      /**
       * 判断key是否存在
       * @param key 键
       * @return true 存在 false不存在
       */
      public boolean hasKey(String key) {
          try {
              return redisTemplate.hasKey(key);
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * 删除缓存
       * @param key 可以传一个值 或多个
       */
      @SuppressWarnings("unchecked")
      public void del(String... key) {
          if (key != null && key.length > 0) {
              if (key.length == 1) {
                  redisTemplate.delete(key[0]);
              } else {
                  redisTemplate.delete(Arrays.stream(key).toList());
              }
          }
      }
  
      //============================String=============================
  
      /**
       * 普通缓存获取
       * @param key 键
       * @return 值
       */
      public Object get(String key) {
          return key == null ? null : redisTemplate.opsForValue().get(key);
      }
  
      /**
       * 普通缓存放入
       * @param key 键
       * @param value 值
       * @return true成功 false失败
       */
      public boolean set(String key, Object value) {
          try {
              redisTemplate.opsForValue().set(key, value);
              return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
  
      }
  
      /**
       * 普通缓存放入并设置时间
       * @param key 键
       * @param value 值
       * @param time 时间(秒) time要大于0 如果time小于等于0 将设置无限期
       * @return true成功 false 失败
       */
      public boolean set(String key, Object value, long time) {
          try {
              if (time > 0) {
                  redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
              } else {
                  set(key, value);
              }
              return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * 递增
       * @param key 键
       * @param delta 要增加几(大于0)
       * @return
       */
      public long increment(String key, long delta) {
          if (delta < 0) {
              throw new RuntimeException("递增因子必须大于0");
          }
          return redisTemplate.opsForValue().increment(key, delta);
      }
  
      /**
       * 递减
       * @param key 键
       * @param delta 要减少几(小于0)
       * @return
       */
      public long decrement(String key, long delta) {
          if (delta < 0) {
              throw new RuntimeException("递减因子必须大于0");
          }
          return redisTemplate.opsForValue().increment(key, -delta);
      }
  
      //================================Map=================================
  
      /**
       * HashGet
       * @param key 键 不能为null
       * @param item 项 不能为null
       * @return 值
       */
      public Object hget(String key, String item) {
          return redisTemplate.opsForHash().get(key, item);
      }
  
      /**
       * 获取hashKey对应的所有键值
       * @param key 键
       * @return 对应的多个键值
       */
      public Map<Object, Object> hmget(String key) {
          return redisTemplate.opsForHash().entries(key);
      }
  
      /**
       * HashSet
       * @param key 键
       * @param map 对应多个键值
       * @return true 成功 false 失败
       */
      public boolean hmset(String key, Map<String, Object> map) {
          try {
              redisTemplate.opsForHash().putAll(key, map);
              return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * HashSet 并设置时间
       * @param key 键
       * @param map 对应多个键值
       * @param time 时间(秒)
       * @return true成功 false失败
       */
      public boolean hmset(String key, Map<String, Object> map, long time) {
          try {
              redisTemplate.opsForHash().putAll(key, map);
              if (time > 0) {
                  expire(key, time);
              }
              return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * 向一张hash表中放入数据,如果不存在将创建
       * @param key 键
       * @param item 项
       * @param value 值
       * @return true 成功 false失败
       */
      public boolean hset(String key, String item, Object value) {
          try {
              redisTemplate.opsForHash().put(key, item, value);
              return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * 向一张hash表中放入数据,如果不存在将创建
       * @param key 键
       * @param item 项
       * @param value 值
       * @param time 时间(秒)  注意:如果已存在的hash表有时间,这里将会替换原有的时间
       * @return true 成功 false失败
       */
      public boolean hset(String key, String item, Object value, long time) {
          try {
              redisTemplate.opsForHash().put(key, item, value);
              if (time > 0) {
                  expire(key, time);
              }
              return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * 删除hash表中的值
       * @param key 键 不能为null
       * @param item 项 可以使多个 不能为null
       */
      public void hdel(String key, Object... item) {
          redisTemplate.opsForHash().delete(key, item);
      }
  
      /**
       * 判断hash表中是否有该项的值
       * @param key 键 不能为null
       * @param item 项 不能为null
       * @return true 存在 false不存在
       */
      public boolean hHasKey(String key, String item) {
          return redisTemplate.opsForHash().hasKey(key, item);
      }
  
      /**
       * hash递增 如果不存在,就会创建一个 并把新增后的值返回
       * @param key 键
       * @param item 项
       * @param by 要增加几(大于0)
       * @return
       */
      public double hincr(String key, String item, double by) {
          return redisTemplate.opsForHash().increment(key, item, by);
      }
  
      /**
       * hash递减
       * @param key 键
       * @param item 项
       * @param by 要减少记(小于0)
       * @return
       */
      public double hdecr(String key, String item, double by) {
          return redisTemplate.opsForHash().increment(key, item, -by);
      }
  
      //============================set=============================
  
      /**
       * 根据key获取Set中的所有值
       * @param key 键
       * @return
       */
      public Set<Object> sGet(String key) {
          try {
              return redisTemplate.opsForSet().members(key);
          } catch (Exception e) {
              e.printStackTrace();
              return null;
          }
      }
  
      /**
       * 根据value从一个set中查询,是否存在
       * @param key 键
       * @param value 值
       * @return true 存在 false不存在
       */
      public boolean sHasKey(String key, Object value) {
          try {
              return redisTemplate.opsForSet().isMember(key, value);
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * 将数据放入set缓存
       * @param key 键
       * @param values 值 可以是多个
       * @return 成功个数
       */
      public long sSet(String key, Object... values) {
          try {
              return redisTemplate.opsForSet().add(key, values);
          } catch (Exception e) {
              e.printStackTrace();
              return 0;
          }
      }
  
      /**
       * 将set数据放入缓存
       * @param key 键
       * @param time 时间(秒)
       * @param values 值 可以是多个
       * @return 成功个数
       */
      public long sSetAndTime(String key, long time, Object... values) {
          try {
              Long count = redisTemplate.opsForSet().add(key, values);
              if (time > 0) {
                  expire(key, time);
              }
              return count;
          } catch (Exception e) {
              e.printStackTrace();
              return 0;
          }
      }
  
      /**
       * 获取set缓存的长度
       * @param key 键
       * @return
       */
      public long sGetSetSize(String key) {
          try {
              return redisTemplate.opsForSet().size(key);
          } catch (Exception e) {
              e.printStackTrace();
              return 0;
          }
      }
  
      /**
       * 移除值为value的
       * @param key 键
       * @param values 值 可以是多个
       * @return 移除的个数
       */
      public long setRemove(String key, Object... values) {
          try {
              Long count = redisTemplate.opsForSet().remove(key, values);
              return count;
          } catch (Exception e) {
              e.printStackTrace();
              return 0;
          }
      }
      //===============================list=================================
  
      /**
       * 获取list缓存的内容
       * @param key 键
       * @param start 开始
       * @param end 结束  0 到 -1代表所有值
       * @return
       */
      public List<Object> lGet(String key, long start, long end) {
          try {
              return redisTemplate.opsForList().range(key, start, end);
          } catch (Exception e) {
              e.printStackTrace();
              return null;
          }
      }
  
      /**
       * 获取list缓存的长度
       * @param key 键
       * @return
       */
      public long lGetListSize(String key) {
          try {
              return redisTemplate.opsForList().size(key);
          } catch (Exception e) {
              e.printStackTrace();
              return 0;
          }
      }
  
      /**
       * 通过索引 获取list中的值
       * @param key 键
       * @param index 索引  index>=0时， 0 表头，1 第二个元素，依次类推；index<0时，-1，表尾，-2倒数第二个元素，依次类推
       * @return
       */
      public Object lGetIndex(String key, long index) {
          try {
              return redisTemplate.opsForList().index(key, index);
          } catch (Exception e) {
              e.printStackTrace();
              return null;
          }
      }
  
      /**
       * 将list放入缓存
       * @param key 键
       * @param value 值
       * @return
       */
      public boolean lSet(String key, Object value) {
          try {
              redisTemplate.opsForList().rightPush(key, value);
              return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * 将list放入缓存
       * @param key 键
       * @param value 值
       * @param time 时间(秒)
       * @return
       */
      public boolean lSet(String key, Object value, long time) {
          try {
              redisTemplate.opsForList().rightPush(key, value);
              if (time > 0) {
                  expire(key, time);
              }
              return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * 将list放入缓存
       * @param key 键
       * @param value 值
       * @return
       */
      public boolean lSet(String key, List<Object> value) {
          try {
              redisTemplate.opsForList().rightPushAll(key, value);
              return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * 将list放入缓存
       * @param key 键
       * @param value 值
       * @param time 时间(秒)
       * @return
       */
      public boolean lSet(String key, List<Object> value, long time) {
          try {
              redisTemplate.opsForList().rightPushAll(key, value);
              if (time > 0) {
                  expire(key, time);
              }
              return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * 根据索引修改list中的某条数据
       * @param key 键
       * @param index 索引
       * @param value 值
       * @return
       */
      public boolean lUpdateIndex(String key, long index, Object value) {
          try {
              redisTemplate.opsForList().set(key, index, value);
              return true;
          } catch (Exception e) {
              e.printStackTrace();
              return false;
          }
      }
  
      /**
       * 移除N个值为value
       * @param key 键
       * @param count 移除多少个
       * @param value 值
       * @return 移除的个数
       */
      public long lRemove(String key, long count, Object value) {
          try {
              Long remove = redisTemplate.opsForList().remove(key, count, value);
              return remove;
          } catch (Exception e) {
              e.printStackTrace();
              return 0;
          }
      }
  }
  ```

#### 整合JWT

- JWT简介
  
  > JWT(JSON Web Token)是一个开放标准(RFC 7519)，它定义了一种紧凑的、自包含的方式，用于作为JSON对象在各方之间安全地传输信息。该信息可以被验证和信任，因为它是数字签名的。

- JWT特点
  
  > 1. 无状态：JWT包含认证信息，token只需要保存在客户端，服务端不需要保存会话信息，所以JWT是无状态的。这一点使得服务器压力大大降低，增加了系统的可用性和可扩展性。但是无状态同时也是JWT最大的缺点，服务器无法主动让token失效。
  > 
  > 2. 安全性：客户端每次请求都会携带token，可以有效避免CSRF攻击，同时token会自动过期，可以减少token被盗用的情况，服务器会通过jwt签名验证token，可以避免token被篡改。
  > 
  > 3. 可见性：JWT的payload部分可以直接通过Base64解码，所以可存储一些其他业务逻辑所必要的非敏感信息。

- 引入JWT依赖
  
  ```xml
      <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt</artifactId>
        <version>0.9.1</version>
      </dependency>
  ```
  
  主要使用jwt来生成token，验证token

- JWT配置
  
  ```yml
  icreator:
    jwt:
      header: authorization
      expire: 300 # 过期时间 单位秒
      secret: F7frIoCneptp5uFnzOc2mAijifRPK4Ed
  ```
  
  我们将JWTUtil中用到的一些配置向自定义在yml配置文件中。

- 为了方便开发我们增加JWTUtil
  
  ```java
  package com.wjl.system.utils;
  
  import io.jsonwebtoken.Claims;
  import io.jsonwebtoken.Jwts;
  import io.jsonwebtoken.SignatureAlgorithm;
  import java.util.Date;
  import org.springframework.boot.context.properties.ConfigurationProperties;
  import org.springframework.stereotype.Component;
  
  @Component
  @ConfigurationProperties(prefix = "icreator.jwt")
  public class JWTUtil {
  
      private long expire;
  
      private String secret;
  
      private String header;
  
      /**
       * 生成token
       * @return
       */
      public String generateToken(String username) {
          Date date = new Date();
          Date expireDate = new Date(date.getTime() + expire * 1000);
  
          String token = Jwts.builder().setHeaderParam("type", "JWT").setSubject(username).setIssuedAt(date)
              .setExpiration(expireDate).signWith(
                  SignatureAlgorithm.HS512, secret).compact();
          return token;
      }
  
      /**
       * 解析jwt
       *
       * @param jwt
       * @return
       */
      public Claims getClaimsByToken(String jwt) {
          try {
              return Jwts.parser().setSigningKey(secret).parseClaimsJws(jwt).getBody();
          } catch (Exception e) {
              //e.printStackTrace();
              return null;
          }
      }
  
      /**
       * token是否过期了
       * @param claims
       * @return
       */
      public boolean tokenExpire(Claims claims) {
          return claims.getExpiration().before(new Date());
      }
  
      public long getExpire() {
          return expire;
      }
  
      public void setExpire(long expire) {
          this.expire = expire;
      }
  
      public String getSecret() {
          return secret;
      }
  
      public void setSecret(String secret) {
          this.secret = secret;
      }
  
      public String getHeader() {
          return header;
      }
  
      public void setHeader(String header) {
          this.header = header;
      }
  }
  ```

#### 引入hutool

- hutool是一个工具类，这里我们可能用到里面的一些方法，我们将它引入到项目中，如果不需要则不用引入，此步骤非必需
  
  ```xml
    <!--hutool-->
      <dependency>
        <groupId>cn.hutool</groupId>
        <artifactId>hutool-all</artifactId>
        <version>5.8.0.M1</version>
      </dependency>
  ```

### 登录功能

基本流程：

> 登录页面->获取验证码图片->用户输入用户名、密码、验证码->校验验证码是否正确->校验用户名、密码是否正确->获取用户权限

#### 验证码校验

##### 获取验证码

> 方法流程：
> 
> 1. 创建一个key
> 
> 2. 生成一个验证码
> 
> 3. 将验证码生成图片
> 
> 4. 将验证码存入redis缓存中，过期时间为5分钟
> 
> 5. 将key和生成的验证码图片字节流用base64编码返回给前端

新建一个获取验证码的controller，里面有一个获取验证码的方法。代码如下：

```java
@RestController
public class LoginController {

    @Autowired
    private Producer producer;

    @Autowired
    private RedisUtil redisUtil;

    @GetMapping("/captcha")
    public Result captcha() throws IOException {
        String captchaKey = UUID.randomUUID().toString();
        String code = producer.createText();

        BufferedImage image = producer.createImage(code);
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        ImageIO.write(image, CaptchaConsts.IMAGE_JPG, outputStream);

        Base64.Encoder encoder = Base64.getEncoder();

        String base64Img = CaptchaConsts.IMAGE_TYPE + encoder.encodeToString(outputStream.toByteArray());

        redisUtil.hset(CaptchaConsts.CAPTCHA_HASH_KEY, captchaKey, code, 300);
        return Result.success(
            MapUtil.builder().put(CaptchaConsts.CAPTCHA_KEY, captchaKey).put(CaptchaConsts.CAPTCHA_BASE64IMAGE,
                base64Img).build());

    }

}
```

我们用到了google的验证码生成类，所以需要引入google验证码jar包

```xml
 <dependency>
      <groupId>com.github.penggle</groupId>
      <artifactId>kaptcha</artifactId>
      <version>2.3.2</version>
    </dependency>
```

##### 校验验证码

用户输入验证码，前端会将验证码和key传给后端服务器，后端通过key从redis中获取出来验证码和用户输入的进行比较，相同则校验通过。否则提示验证码错误。

我们使用的是spring security，所以我们将校验验证码写成一个filter，在校验用户名、密码的filter之前执行。

captchaFilter代码：

```java
package com.wjl.system.security;

import com.wjl.common.CaptchaException;
import com.wjl.consts.CaptchaConsts;
import com.wjl.system.utils.RedisUtil;
import java.io.IOException;
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;
import org.springframework.web.filter.OncePerRequestFilter;

@Component
public class CaptchaFilter extends OncePerRequestFilter {

    @Autowired
    private RedisUtil redisUtil;

    @Autowired
    private LoginFailureHandler loginFailureHandler;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
        throws ServletException, IOException {
        String url = request.getRequestURI();
        if (url.equals("/login") && request.getMethod().equals("POST")) {
            try {
                validateCaptcha(request);
            } catch (CaptchaException e) {
                loginFailureHandler.onAuthenticationFailure(request, response, e);
                return;
            }
        }
        filterChain.doFilter(request, response);
    }

    private void validateCaptcha(HttpServletRequest request) throws CaptchaException, IOException {
        String key = request.getParameter(CaptchaConsts.CAPTCHA_KEY);
        String code = request.getParameter(CaptchaConsts.CAPTCHA_VALUE);
        if (!StringUtils.hasText(code) || !StringUtils.hasText(key)) {
            throw new CaptchaException("验证码错误");
        }
        if (!code.equals(redisUtil.hget(CaptchaConsts.CAPTCHA_HASH_KEY, key))) {
            throw new CaptchaException("验证码错误");
        }
        redisUtil.hdel(CaptchaConsts.CAPTCHA_HASH_KEY, key);
    }

}
```

需要创建一个Security配置类来配置Spring Security

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @autowire
    private CaptchaFilter captchaFilter;
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        /*允许跨域*/
        http.cors().
            and().csrf().disable()
            .addFilterBefore(captchaFilter, UsernamePasswordAuthenticationFilter.class);
    }
}
```

##### 测试

编写测试类来测试验证码方法

```java
@SpringBootTest
@RunWith(SpringRunner.class)
@AutoConfigureMockMvc
public class LoginTests {

    @Autowired
    ISystemUserService userService;

    @Autowired
    private MockMvc mockMvc;

        @Test
    public void testGetCaptcha() throws Exception {
        String res = mockMvc.perform(MockMvcRequestBuilders.get("/captcha"))
            .andExpect(MockMvcResultMatchers.status().isOk()).andReturn().getResponse().getContentAsString(
                StandardCharsets.UTF_8);
        System.out.println(res);
    }
}
```

#### 用户名密码校验

我们需要实现UserDetailsService这个接口来实现从数据库中查询用户名、密码。默认Security是从内存中获取用户名、密码进行校验，不符合我们的要求所以需要实现接口自己重写loadUserByUsername这个方法

```java
@Component
public class UserDetailServiceImpl implements UserDetailsService {

    @Autowired
    private ISystemUserService userService;

    // 认证
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        SystemUser user= userService.getOne(new QueryWrapper<SystemUser>().eq("username", username));
        if(null == user){
            throw new UsernameNotFoundException("用户名或密码错误");
        }
        return new User(username, user.getPassword(), null);
    }

}
```

将UserDetailService配置到我们的Security中，这样它才知道用我们这个service实现来执行。

```java
@autowire
private UserDetailsServiceImpl userDetailsService;


// 重写方法配置userDetailsService
 @Override
 protected void configure(AuthenticationManagerBuilder auth) throws Exception {
      auth.userDetailsService(userDetailsService);
 }
```

测试

```java
 @Test
    public void testLogin() throws Exception {
        String username = "guest";
        String password = "111111";
        String verifyCode = "3n4ex";
        String key = "b0251f5d-a414-4b64-9764-22e83bcb336e";
        String res = mockMvc.perform(MockMvcRequestBuilders.post("/login").param("username", username)
                .param("password", password).param("verifyCode", verifyCode)
                .param("key", key))
            .andExpect(MockMvcResultMatchers.status().isOk()).andReturn().getResponse().getContentAsString();
        System.out.println(res);
    }
```

密码加密

这时候密码是没有加密的，我们需要指定加密方式然后再保存到数据库，但是这样加密方式是明文，这种不太好了，所以我们推荐使用brcypt加密方式。在security类中配置：

```java
    @Bean
    BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }
```

#### 登录成功处理

> 登录成功之后要将JWT生成的token存到redis中，然后再返回给前端，后面前端每一次的请求都要带着token信息。登录成功需要我们创建一个自己的Filter来处理我们的业务逻辑

```java
package com.wjl.system.security;

import cn.hutool.json.JSONUtil;
import com.wjl.common.Result;
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServletResponse;

public class LoginBaseHandler {

    protected void responseJson(HttpServletResponse response, Result result) throws IOException {
        response.setContentType("application/json;charset=UTF-8");
        ServletOutputStream servletOutputStream = response.getOutputStream();

        servletOutputStream.write(JSONUtil.toJsonStr(result).getBytes(StandardCharsets.UTF_8));

        servletOutputStream.flush();
        servletOutputStream.close();
    }
}
```

```java
package com.wjl.system.security;

import cn.hutool.json.JSONUtil;
import com.wjl.common.Result;
import com.wjl.consts.UserConsts;
import com.wjl.system.utils.JWTUtil;
import com.wjl.system.utils.RedisUtil;
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.Authentication;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.stereotype.Component;

@Component
public class LoginSuccessHandler extends LoginBaseHandler implements AuthenticationSuccessHandler {

    @Autowired
    private JWTUtil jwtUtil;

    @Autowired
    private RedisUtil redisUtil;

    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response,
        Authentication authentication) throws IOException, ServletException {
        String username = authentication.getName();
        String token = jwtUtil.generateToken(username);
        redisUtil.set(UserConsts.USER_TOKEN_REDIS_KEY_PRE + username, token, jwtUtil.getExpire());
        this.responseJson(response, Result.success("登录成功", JSONUtil.createObj().set("token", token)));
    }
}
```

#### 登录失败处理

> 登录失败返回对应的错误信息

```java
package com.wjl.system.security;

import com.wjl.common.Result;
import java.io.IOException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.stereotype.Component;

@Component
public class LoginFailureHandler extends LoginBaseHandler implements AuthenticationFailureHandler {

    @Override
    public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response,
        AuthenticationException exception) throws IOException {

        this.responseJson(response, Result.fail("用户名或密码错误"));
    }
}
```

#### 鉴权

定义子类来实现鉴权功能（也就是将用户所对应的权限传给spring security的鉴权类中），继承BasicAuthenticationFilter。每一次请求都会进入这个filter。

实现流程：

1. 检查前端携带token是否正确

2. 检查token是否过期

3. 重置token的过期时间

4. 但是这里有一个遗留问题：**用户如果JWT过期了，就需要重新登录。这个问题还是需要解决的**

```java
package com.wjl.system.security;

import cn.hutool.core.util.StrUtil;
import com.baomidou.mybatisplus.core.toolkit.ObjectUtils;
import com.wjl.consts.UserConsts;
import com.wjl.system.entity.SystemUser;
import com.wjl.system.service.ISystemUserService;
import com.wjl.system.utils.JWTUtil;
import com.wjl.system.utils.RedisUtil;
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.JwtException;
import java.io.IOException;
import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.www.BasicAuthenticationFilter;

public class JwtAuthenticationFilter extends BasicAuthenticationFilter {

    @Autowired
    private JWTUtil jwtUtil;

    @Autowired
    private UserDetailsServiceImpl userDetail;

    @Autowired
    private ISystemUserService userService;

    @Autowired
    private RedisUtil redisUtil;

    public JwtAuthenticationFilter(AuthenticationManager authenticationManager) {
        super(authenticationManager);
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
        throws IOException, ServletException {

        String jwt = request.getHeader(jwtUtil.getHeader());
        if (StrUtil.isBlankOrUndefined(jwt)) {
            chain.doFilter(request, response);
            return;
        }
        Claims claims = jwtUtil.getClaimsByToken(jwt);
        if (claims == null) {
            throw new JwtException("token 异常");
        }

        String username = claims.getSubject();
        if (!redisUtil.hasKey(UserConsts.USER_TOKEN_REDIS_KEY_PRE + username)) {
            throw new JwtException("token 过期");
        }
        redisUtil.set(UserConsts.USER_TOKEN_REDIS_KEY_PRE + username, jwt, jwtUtil.getExpire());

        // 通过username获取user
        SystemUser systemUser = userService.getSystemUserByUsername(username);
        if(ObjectUtils.isNull(systemUser)){
            throw new RuntimeException("用户未登录");
        }

        UsernamePasswordAuthenticationToken token = new UsernamePasswordAuthenticationToken(username, null,
            userDetail.getUserGrantedAuthority(systemUser.getId()));
        SecurityContextHolder.getContext().setAuthentication(token);

        chain.doFilter(request, response);
    }
}
```

```java
package com.wjl.system.security;

import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.wjl.system.entity.SystemUser;
import com.wjl.system.service.ISystemUserService;
import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Component;

@Component
public class UserDetailsServiceImpl implements UserDetailsService {

    @Autowired
    private ISystemUserService userService;

    // 认证
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        SystemUser user= userService.getOne(new QueryWrapper<SystemUser>().eq("username", username));
        if(null == user){
            throw new UsernameNotFoundException("用户名或密码错误");
        }
        return new User(username, user.getPassword(), getUserGrantedAuthority(user.getId()));
    }

    // 鉴权
    public List<GrantedAuthority> getUserGrantedAuthority(int userId){
        String commaAuthority = userService.getAuthorityInfo(userId);
        return AuthorityUtils.commaSeparatedStringToAuthorityList(commaAuthority);
    }
}
```

UserDetails类最终代码如上。

#### 退出（注销）功能

> 退出需要我们将用户信息从redis中删除，这样用户再次使用之前的token来登录的话会提示token过期，重新登录。

```java
package com.wjl.system.security;

import com.wjl.common.Result;
import com.wjl.consts.UserConsts;
import com.wjl.system.utils.JWTUtil;
import com.wjl.system.utils.RedisUtil;
import io.jsonwebtoken.Claims;
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.Authentication;
import org.springframework.security.web.authentication.logout.LogoutSuccessHandler;
import org.springframework.security.web.authentication.logout.SecurityContextLogoutHandler;
import org.springframework.stereotype.Component;

@Component
public class JwtLogoutSuccessHandler extends LoginBaseHandler implements LogoutSuccessHandler {

    private final JWTUtil jwtUtil;

    private final RedisUtil redisUtil;

    public JwtLogoutSuccessHandler(JWTUtil jwtUtil, RedisUtil redisUtil) {
        this.jwtUtil = jwtUtil;
        this.redisUtil = redisUtil;
    }

    @Override
    public void onLogoutSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication)
        throws IOException, ServletException {

        if(authentication != null){
            new SecurityContextLogoutHandler().logout(request, response, authentication);
        }
        // 将缓存的权限删除
        response.setHeader(jwtUtil.getHeader(), null);
        redisUtil.del(UserConsts.USER_TOKEN_REDIS_KEY_PRE+authentication.getName());
        this.responseJson(response, Result.success());
    }
}
```

#### 最终配置类

```java
package com.wjl.system.config;

import com.wjl.system.security.CaptchaFilter;
import com.wjl.system.security.JwtAccessDeniedHandler;
import com.wjl.system.security.JwtAuthenticationEntryPoint;
import com.wjl.system.security.JwtAuthenticationFilter;
import com.wjl.system.security.JwtLogoutSuccessHandler;
import com.wjl.system.security.LoginFailureHandler;
import com.wjl.system.security.LoginSuccessHandler;
import com.wjl.system.security.UserDetailsServiceImpl;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    private static final String[] URL_WHITELIST = {"/login", "/logout", "/captcha", "/favicon.ico"};

    private final CaptchaFilter captchaFilter;

    private final LoginFailureHandler loginFailureHandler;

    private final LoginSuccessHandler loginSuccessHandler;

    private final JwtAccessDeniedHandler accessDeniedHandler;

    private final JwtAuthenticationEntryPoint authenticationEntryPoint;

    private final UserDetailsServiceImpl userDetailsService;

    private final JwtLogoutSuccessHandler logoutSuccessHandler;

    public SecurityConfig(CaptchaFilter captchaFilter,
        LoginFailureHandler loginFailureHandler, LoginSuccessHandler loginSuccessHandler,
        JwtAccessDeniedHandler accessDeniedHandler, JwtAuthenticationEntryPoint authenticationEntryPoint,
        UserDetailsServiceImpl userDetailsService, JwtLogoutSuccessHandler logoutSuccessHandler) {
        this.captchaFilter = captchaFilter;
        this.loginFailureHandler = loginFailureHandler;
        this.loginSuccessHandler = loginSuccessHandler;
        this.accessDeniedHandler = accessDeniedHandler;
        this.authenticationEntryPoint = authenticationEntryPoint;
        this.userDetailsService = userDetailsService;
        this.logoutSuccessHandler = logoutSuccessHandler;
    }

    @Bean
    JwtAuthenticationFilter jwtAuthenticationFilter() throws Exception {
        return new JwtAuthenticationFilter(authenticationManager());
    }

    @Bean
    BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        /*允许跨域*/
        http.cors().
            and().csrf().disable()
            /*登录*/
            .formLogin()
            .successHandler(loginSuccessHandler)
            .failureHandler(loginFailureHandler)

            /*退出*/
            .and()
            .logout()
            .logoutSuccessHandler(logoutSuccessHandler)

            /* 关闭session */
            .and()
            .sessionManagement()
            .sessionCreationPolicy(SessionCreationPolicy.STATELESS)

            /* 认证放行 */
            .and()
            .authorizeRequests()
            .antMatchers(URL_WHITELIST).permitAll()
            .anyRequest().authenticated()
            /* 配置自定义过滤器 */
            .and()
            .addFilter(jwtAuthenticationFilter())
            .addFilterBefore(captchaFilter, UsernamePasswordAuthenticationFilter.class)
            /*异常处理*/
            .exceptionHandling().authenticationEntryPoint(authenticationEntryPoint)
            .accessDeniedHandler(accessDeniedHandler);

    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService);
    }
}
```

#### 在启动类中打开redis缓存

```java
package com.wjl.system;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cache.annotation.EnableCaching;

@SpringBootApplication
@EnableCaching
public class SystemApplication {

    public static void main(String[] args) {
        SpringApplication.run(SystemApplication.class, args);
    }

}
```

自定义sql

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wjl.system.mapper.SystemUserMapper">

  <select id="getNavMenu" resultType="com.wjl.system.entity.extend.UserRoleAuth">
    SELECT
      distinct
      r.name roleName,
      r.`code` roleCode,
      a.id authId,
      a.`name` authName,
      a.label authLabel,
      a.`code` authCode,
      a.icon authIcon,
      a.url authUrl,
      a.type authType,
      a.status authStatus
    FROM
      system_role r,
      system_user_role ur,
      system_role_authorization ra,
      system_authorization a
    WHERE
        r.id=ur.role_id
    AND ur.role_id = ra.role_id
    AND ra.authorization_id = a.id
    AND ur.user_id = #{userId}
  </select>
</mapper>
```

#### 自定义权限校验方法

#### 实现接口Security

##### 定义方法

```java
@Component("ex")
public class SGExpressionRoot{
    public boolean hasAuthority(String authority){
        // 从SecurityContentHolder.getContext().getAuthentication();
    }
}
```

##### 调用

@PreAuthorize("ex.hasAuthority('权限')")

#### 配置方式配置权限

在SecurityConfig类中进行配置

## 跨域问题解决

原因：

> 浏览器出于安全的考虑，使用XMLHttpRequest对象发起HTTP请求时必须遵守同源策略，否则就时跨域的HTTP请求，默认情况下是被禁止的。同源策略要求源相同才能正常进行通信，即协议、域名、端口都完全一致。

现状：

> 前后端分离项目，前端项目和后端项目一般都不是同源的，所以肯定会存在跨域请求的问题。

解决方案：

> 跨域有多种解决方式，大致是全局配置、重写WebMvcConfigurer、过滤器配置、Controller中配置注解@CrossOrigin、nginx反向代理
> 
> 我们使用第一种全局配置定义CorsFilter的方式，新建一个类，设置为配置类。代码如下

```java
package com.wjl.system.config;


import com.wjl.consts.CorsConsts;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.filter.CorsFilter;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Bean
    public CorsFilter corsFilter() {
        final UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        final CorsConfiguration config = new CorsConfiguration();

        config.setAllowCredentials(true);
        config.addAllowedOriginPattern(CorsConsts.WHITE_MAIN_URL);
        config.addAllowedHeader(CorsConfiguration.ALL);
        config.addAllowedMethod(CorsConfiguration.ALL);
        config.addExposedHeader(CorsConsts.EXPOSED_HEADER);
        source.registerCorsConfiguration(CorsConsts.REGISTER_CORS_PATTERN_ALL, config);
        return new CorsFilter(source);
    }

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping(CorsConsts.REGISTER_CORS_PATTERN_ALL).allowedOrigins(CorsConfiguration.ALL)
            .allowedMethods("GET", "POST", "DELETE", "PUT").maxAge(3600);
    }
}
```

- 这里指定只能http://localhost:8080来跨域访问。安全性更高一点，如果安全性再高一点的话可以设置header，method

- 在security的配置中有一个http.cors()设置的
  
  ---
  
  **总结**：
  
  **上面代码配置是springboot的跨域配置，下面http.cors是开启spring-security跨域** 

## 用户管理

## 角色管理

## 权限管理
