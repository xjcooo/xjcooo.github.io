---
title: spring-security系列(一)简介
date: 2019-07-23 14:08:03
categories: spring-security系列
tags:
- spring
- spring security
typora-root-url: ../
---

# SpringSecurity简介

## 核心功能

1. 认证

   验证某一用户是否是系统内的合法主体, 也就是用户能否访问该系统, 一般是通过验证用户账号及密码的方式来完成认证过程

2. 授权

3. 攻击防护（防止伪造身份）

## 简介

### 原理

	对Web资源进行保护，最好的办法莫过于Filter，要想对方法调用进行保护，最好的办法莫过于AOP。所以springSecurity在我们进行用户认证以及授予权限的时候，通过各种各样的拦截器来控制权限的访问，从而实现安全。
- 主要过滤器 
    `WebAsyncManagerIntegrationFilter `
    `SecurityContextPersistenceFilter `
    `HeaderWriterFilter` 
    `CorsFilter `
    `LogoutFilter`
    `RequestCacheAwareFilter`
    `SecurityContextHolderAwareRequestFilter`
    `AnonymousAuthenticationFilter`
    `SessionManagementFilter`
    `ExceptionTranslationFilter`
    `FilterSecurityInterceptor`
    `UsernamePasswordAuthenticationFilter`
    `BasicAuthenticationFilter`
### 框架的核心组件
    `SecurityContextHolder`：提供对`SecurityContext`的访问
    `SecurityContext`：持有`uthentication`对象和其他可能需要的信息
    `AuthenticationManager` 其中可以包含多个`AuthenticationProvider`
    `ProviderManager`对象为`AuthenticationManager`接口的实现类
    `AuthenticationProvider` 主要用来进行认证操作的类 调用其中的`authenticate()`方法去进行认证操作
    `Authentication`：`Spring Security`方式的认证主体
    `GrantedAuthority`：对认证主题的应用层面的授权，含当前用户的权限信息，通常使用角色表示
    `UserDetails`：构建`Authentication`对象必须的信息，可以自定义，可能需要访问DB得到
    `UserDetailsService`：通过`username`构建`UserDetails`对象，通过`loadUserByUsername`根据`userName`获取`UserDetail`对象 （可以在这里基于自身业务进行自定义的实现  如通过数据库，`xml`,缓存获取等） 

## 自定义安全配置

### 基于业务需求定义方案

​	自定义一个`springSecurity`安全配置类, 继承`WebSecurityConfigurerAdapter`,重写`configure(HttpSecurity http)`方法来重定义`http`请求, 重写`configure(WebSecurity web)`方法来配置`web`安全配置, 重写`configure(AuthenticationManagerBuilder auth)`方法来定义用户认证方式.

