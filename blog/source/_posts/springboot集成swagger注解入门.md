---
title: springboot集成swagger注解入门
date: 2018-01-03 14:46:49
tags: Swagger
---
#### [Swagger][1]简介 THE WORLD'S MOST POPULAR API TOOLING
Swagger is the world’s largest framework of API developer tools for the OpenAPI Specification(OAS), enabling development across the entire API lifecycle, from design and documentation, to test and deployment.

#### Swagger的作用:

Swagger是一款RESTFUL接口的文档在线自动生成框架，该框架可以自动生成接口文档，也可以进行接口测试，生成mock数据等。通过使用该框架可以快速、动态的更新和管理API，方便前、后台协同开发，提高开发效率，减少开发过程中一些人为的误差。	Swagger让部署管理和使用功能强大的API从未如此简单。

#### SpringBoot 集成Swagger步骤:

1. 引入maven坐标配置

2. 建立Swagger配置

3. 使用Swagger注解

4. 访问API文档查看效果

**引入maven坐标：**

	<dependency>
		<groupId>io.springfox</groupId>
		<artifactId>springfox-swagger2</artifactId>
		<version>${springfox-version}</version>
	</dependency>
	<dependency>
		<groupId>io.springfox</groupId>
		<artifactId>springfox-swagger-ui</artifactId>
		<version>${springfox-version}</version>
	</dependency>
	<dependency>
		<groupId>joda-time</groupId>
		<artifactId>joda-time</artifactId>
		<version>2.9.9</version>
	</dependency>

** 建立Swagger配置类： **

	package com.example.swagger.config;

	import org.springframework.context.annotation.Bean;
	import org.springframework.context.annotation.Configuration;
	import springfox.documentation.builders.ApiInfoBuilder;
	import springfox.documentation.builders.PathSelectors;
	import springfox.documentation.service.ApiInfo;
	import springfox.documentation.spi.DocumentationType;
	import springfox.documentation.spring.web.plugins.Docket;
	import springfox.documentation.swagger2.annotations.EnableSwagger2;
	
	/**
	 * Description: swagger配置类，通过Configuration注解让spring容器自动加载该类
	 *
	 * @author Jiankun.Zhangsun 2018/1/2 15:13
	 */
	@Configuration
	@EnableSwagger2
	public class SwaggerConfig {
	
	    /**
	     * 将返回值当作一个bean注入到容器中
	     *
	     * @return api文档对象
	     */
	    @Bean
	    public Docket createRestApi() {
	        return new Docket(DocumentationType.SWAGGER_2)
	                .apiInfo(apiInfo()).useDefaultResponseMessages(false)
	                .select()
	                .paths(PathSelectors.any())
	                .build();
	    }
	
	    /**
	     * api 文档信息对象，主要是文档标题相关的信息
	     *
	     * @return 文档信息对象
	     */
	    private ApiInfo apiInfo() {
	        return new ApiInfoBuilder()
	                .title("Spring Boot中使用Swagger2构建RESTful APIs")
	                .description("更多Spring Boot相关文章请关注：http://blog.didispace.com/")
	                .termsOfServiceUrl("http://blog.didispace.com/")
	                .contact("Murphy's BLOG")
	                .version("1.0.0.0")
	                .build();
	    }
	}

代码解释： 
ApiInfo 这个对象是用来生成文档相关信息的，通过这个对象可以构建出文档标题、描述、版本等信息；
createRestApi() 这个方法是进行Docket创建的；
Docket 这个对象是用来渲染整个API文档的对象，这个对象通过@Bean注解注册到了Spring容器中，Swagger会自动加载该对象进行文档构建。

** 使用Swagger注解： **

	package com.example.swagger.controller;
	
	import io.swagger.annotations.*;
	import org.springframework.web.bind.annotation.*;
	
	/**
	 * Description: swagger controller test
	 *
	 * @author Jiankun.Zhangsun 2018/1/2 15:40
	 */
	@Api(value = "petApi", description = "pet api description")
	@RestController
	public class PetApi {
	    @ApiOperation(value = "get city by state", notes = "Get city by state", 
	            responseContainer = "container",
	            httpMethod = "get",
				code = 304, 
	            hidden = false, 
	            produces = "application/json;charset=utf-8",
	            response = Pet.class, 
	            tags = {"user", "person", "pet"})
	    @ApiResponses(value = {@ApiResponse(code = 405, message = "Invalid input", response = Pet.class) })
	    @RequestMapping(value = "/pet", method = RequestMethod.GET)
	    public Pet getPet(@ApiParam(value = "param introduction", name = "name", required = true) @RequestBody Pet p) {
	        Pet pet = new Pet();
	        pet.setName(p.getName());
	        pet.setAge(p.getAge());
	        return pet;
	    }
	}

代码解释：
@Api(value, description)

	类级别的注解，用于标识这是一个Api文档类，
	value： 表示Api文档名称，
	description： 用于简单的描述该Api文档。
@ApiOperation(value，notes，response，tags，hidden，httpMethod，consumer...)， 

	value：api功能描述， 
	notes: api功能的详细描述 
	response: 返回值类型的定义，class类型
	tags: api分类的管理，是一个字符串数组对象，每个值对应一个标签目录，没有就会新建一个，有了就直接进行分类
	hidden: 是否隐藏该Api，默认为false， 
	httpMethod: 请求方式
	consumer: 接收的请求头信息
	produces： 生产的请求头信息

@ApiParam(value, name, required)

	value: 参数说明
	name: 参数名称
	required: 是否必传

** 访问API文档查看效果： **

本地访问路径：http://localhost:8081/swagger-ui.html#/ 即可查看生成的效果。










[1]: https://swagger.io/


