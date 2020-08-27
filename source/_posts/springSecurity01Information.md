---
title: spring-security系列(一)简介
date: 2019-07-23 14:08:03
Typora-root-url: ../
categories: spring-security系列
tags:
- spring
- spring security
---

# SpringSecurity简介

## 核心功能

1. 认证

   验证某一用户是否是系统内的合法主体, 也就是用户能否访问该系统, 一般是通过验证用户账号及密码的方式来完成认证过程

2. 授权

3. 攻击防护（防止伪造身份）

## 简介

### 原理

```text
对Web资源进行保护，最好的办法莫过于Filter，要想对方法调用进行保护，最好的办法莫过于AOP。所以springSecurity在我们进行用户认证以及授予权限的时候，通过各种各样的拦截器来控制权限的访问，从而实现安全。
```
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
`SecurityContext`：持有`Authentication`对象和其他可能需要的信息
`AuthenticationManager` 其中可以包含多个`AuthenticationProvider`
`ProviderManager`对象为`AuthenticationManager`接口的实现类
`AuthenticationProvider` 主要用来进行认证操作的类 调用其中的`authenticate()`方法去进行认证操作
`Authentication`：`Spring Security`方式的认证主体
`GrantedAuthority`：对认证主题的应用层面的授权，含当前用户的权限信息，通常使用角色表示
`UserDetails`：构建`Authentication`对象必须的信息，可以自定义，可能需要访问DB得到
`UserDetailsService`：通过`username`构建`UserDetails`对象，通过`loadUserByUsername`根据`userName`获取`UserDetail`对象 （可以在这里基于自身业务进行自定义的实现  如通过数据库，`xml`,缓存获取等） 

## 自定义配置

### 可定制化配置

- WebSecurityConfigurerAdapter

​	自定义一个`springSecurity`安全配置类, 继承`WebSecurityConfigurerAdapter`,重写`configure(HttpSecurity http)`方法来重定义`HttpSecurity`; 重写`configure(WebSecurity web)`方法来配置`WebSecurity`; 重写`configure(AuthenticationManagerBuilder auth)`方法来自定义`AuthenticationManagerBuilder`, 进行认证相关的配置, 等等

​	**`HttpSecurity`配置有如下几类:**

	1. authorizeRequests()配置路径拦截,以及权限和角色认证信息
 	2. formLogin()是表单相关的配置
 	3. httpBasic()用来配置basic登录
 	4. logout()用于配置注销的相关配置
 	5. 等等, 待续

各类配置间使用`and()`连接,并且返回`HttpSecurity`本身, 因此可以连续进行配置.

**`AuthenticationManagerBuilder`配置方式:**

```java
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication()
            .passwordEncoder(new BCryptPasswordEncoder())
            .withUser("admin")
            .password(new BCryptPasswordEncoder().encode("admin"))
            .roles("ADMIN");
    }
}
```

除此通过重写`configure`方法外, 还可以通过`@Autowired`注入全局的`AuthenticationManagerBuilder`的方式来构建, 该方法适用于多个`WebSecurityConfigurerAdapter`的情况, 如:

```java
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
    	auth.inMemoryAuthentication()
            .passwordEncoder(new BCryptPasswordEncoder())
            .withUser("admin")
            .password(new BCryptPasswordEncoder().encode("admin"))
            .roles("ADMIN");
    }
}
```



- UserDetails

用户详情, 通过实现该接口自定义用户实体类

```java
public interface UserDetails extends Serializable {

   Collection<? extends GrantedAuthority> getAuthorities();

   String getPassword();

   String getUsername();

   boolean isAccountNonExpired();

   boolean isAccountNonLocked();

   boolean isCredentialsNonExpired();

   boolean isEnabled();
}
```



- UserDetailsService

用户信息加载方法, 通过实现该接口,来加载用户信息

```java
public interface UserDetailsService {
   UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
}
```

该接口常见实现类有: `JdbcDaoImpl`, `InMemoryUserDetailsManager`