# Javaweb



# 一、基础知识补充

### 1、xml

```
 用于数据库链接等数据规范格式
 格式如下，可直接用web框架导入
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
</web-app>
```



### 2、tomcat

```
1、下载安装：
下载解压到有权限的文件夹中

2、配置：
系统变量修改：
JAVA_HOME
C:\Program Files\Java\jdk-17
CATALINA_HOME
D:\procedure\java\apache-tomcat-10.1.26

path：
%JAVA_HOME%\bin
%CATALINA_HOME%\bin

修改日志成中文GBK：
\apache-tomcat-10.1.26\conf\logging

3、项目开启
第一种：
直接在tomcat中新建项目结构

第二种：
在D:\procedure\java\apache-tomcat-10.1.26\conf\Catalina\localhost新建app.xml索引文件
<Context path="/app" docBase="新建的项目结构真实路径" />

第三种：
关联idea进行coding
idea中设置应用程序服务器导入tomcat
在项目引入tomcat依赖
项目编辑完后进行构建，并且比在运行窗口进行配置，修改url


```



### 3、servlet

#### 生命周期

```java
Servlet 运行在 Web 服务器上，可以接收来自客户端的 HTTP 请求，并生成相应的 HTTP 响应
    
 Servlet生命周期
    1	实例化		构造器		第一次请求/服务启动		1
    2	初始化		init		构造完毕		1
    3	服务		service		每次请求		多次
    4	销毁		destory		关闭服务		1
    
 Servlet是单例，成员变量在多个线程池之中是共享的
 default-servlet用于加载静态资源，默认随服务启动，序号为1
    
```

#### 继承结构

```java
// Servlet基础类
public interface Servlet {
    // 初始化方法，构造完毕 -动调用完成初始化功能的方法
    void init(ServletConfig var1) throws ServletException;
	
    // 获得ServletConfig对象的方法
    ServletConfig getServletConfig();

    // 接受用户请求，用于响应信息的方法
    void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;

    // 返回Servlet字符串形式描述信息的方法
    String getServletInfo();
	
    // 销毁方法，资源释放的工作
    void destroy();
}
```



#### 请求转发

```
请求转发通过HttpServletRequest对象获取请求转发器实现
请求转发是服务器内部的行为,对客户端是屏蔽的
客户端只发送了一次请求,客户端地址栏不变
服务端只产生了一对请求和响应对象,这一对请求和响应对象会继续传递给下一个资源
因为全程只有一个HttpServletRequset对象,所以请求参数可以传递,请求域中的数据也可以传递
请求转发可以转发给其他Servlet动态资源,也可以转发给一些静态资源以实现页面跳转
请求转发可以转发给WEB-INF下受保护的资源
请求转发不能转发到本项目以外的外部资源
```



#### 响应重定向

```
响应重定向通过HttpServletResponse对象sendRedirect方法实现
响应重定向是服务端通过302响应码和路径,告诉客户端自己去找其他资源,是在服务端提示下的,客户端的行为
客户端至少发送了两次请求,客户端地址栏是要变化的
服务端产生了多对请求和响应对象,且请求和响应对象不会传递给下一个资源
因为全程产生了多个HttpServletRequset对象,所以请求参数不可以传递,请求域中的数据也不可以传递
重定向可以是其他Servlet动态资源,也可以是一些静态资源以实现页面跳转
重定向不可以到给WEB-INF下受保护的资源
重定向可以到本项目以外的外部资源
```

 
