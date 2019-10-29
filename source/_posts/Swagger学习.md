---
title: Swagger学习
date: 2019-10-19 14:34:52
tags:
---
## Swagger  
### 学习目标  
- 了解Swagger的作用和概念
- 了解前后端分离
- 在SpringBoot中集成Swagger
## Swagger简介  
### 前后端分离
- Vue+SpringBoot
- 后端时代：前端只用管理静态页面；html==>后端。模板引擎JSP=>后端的主力

前后端分离时代：
- 后端：后端控制层，服务层，数据访问层【后端团队】
- 前端：前端控制层，视图层【前端团队】
 - 伪造后端数据，json，已经存在，不需要后端前端工程依旧能跑起来
- 前后端如何交互？==>API接口
- 前后端相对独立，松耦合
-前后端甚至可以部署在不同服务器上

产生一个问题
- 前后端集成联调，前端人员和后端人员无法做到”即时协商，尽早解决“，最终导致问题集中爆发；

解决方案：
- 首先指定schema【计划提纲】 实时更新最新API，降低集成风险；
- 早些年：指定word计划文档；
- 前后端分离：
 - 前端测试后端接口:postman
 - 后端提供接口，需要实时更新最新的消息及改动！

### Swagger
- 号称世界上最流行的api框架
- RestFul Api文档在线自动生成工具=>Api文档与`Api定义同步更新`
- 可以直接运行，可以在线测试API接口
- 支持多种语言：（Java，Php）

[官网](https://swagger.io/)
在项目中使用Swagger需要jar包springbox；
- swagger2
- ui
## SpringBoot集成Swagger
1. 新建一个SpringBoot-WAB项目
2. 导入相关依赖[链接](https://mvnrepository.com/search?q=springfox) 
```
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```
3. 编写一个Hello工程
4. 集成Swagger=>Config
```
@Configuration
@EnableSwagger2 //开启Swagger2新版
public class SwaggerConfig {
}
```
5. 测试运行[网址](http://localhost:8080/swagger-ui.html)

## 配置Swagger  
###  Swagger的bean实例Docket,是否启动，扫描接口
```
@Configuration
@EnableSwagger2 //开启Swagger2新版
public class SwaggerConfig {
    //配置了Swagger的Docket实例
    @Bean
    public Docket docket(Environment environment) {
        //设置要显示的swagger环境
        Profiles profiles = Profiles.of("dev","test");
        //获取项目的环境
        boolean flag = environment.acceptsProfiles(profiles);

        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .groupName("sun")
                //enable是否启动swagger，如果为false，则swagger不能在浏览器中访问
                .enable(flag)
                .select()
                //RequestHandlerSelectors配置扫描接口的方式
                //basePackage指定要扫描的包
                //any()扫描全部
                //none()都不扫描
                //withClassAnnotation扫描类上的注解
                .apis(RequestHandlerSelectors.basePackage("com.sun.swagger.controller"))
                //paths过滤什么路径
                //.paths(PathSelectors.ant("/sun/**"))
                .build()
                ;
    }
```
### Swagger配置信息
```
  //配置Swagger信息=apiInfo
    private ApiInfo apiInfo() {
        //作者信息
        Contact contact = new Contact("孙文道", "http://blog.kuangstudy.com/", "1243055060@qq.com");
        return new ApiInfo("文道的SwaggerAPI文档",
                "开源是男人的浪漫",
                "1.0",
                "http://blog.kuangstudy.com/",
                contact,
                "Apache 2.0",
                "http://www.apache.org/licenses/LICENSE-2.0",
                new ArrayList()
        );
    }
```

## 总结
1. 我们可以通过swagger给一些比较难理解的属性或接口增加注释信息
2. 接口文档实时更新
3. 可以在线测试

【注意点】在正式发布的时候，关闭swagger！！！

