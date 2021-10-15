# Spring Framework

![img](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/spring-framework.png)

## What is Spring?

Spring is a open-source flexible application framework, which has multi-layers and supporting integration with different components and frameworks.

## Features

- IoC - Inversion of Control
  - DI - Dependency Injection
- AOP - Aspect Oriented Programming
- Container - create and manage Beans
- MVC - work with DispatcherServlet to handle requests
- Transaction Management

# IoC

## Definition

A process that objects define their dependencies only through constructor arguments or arguments to a factory method. Spring IoC container will inject those dependencies when the Bean is created.

IoC can be implemented via Dependency Injection (DI) or Dependency Lookup (DL).

## How to inject dependency?

### Interface Injection

``` java
public class ClassA {
  private InterfaceB interfaceBImpl;
  public void doSomething() {
    Object obj = Class.forName(Config.InterfaceBImpl).newInstance();
    interfaceBImpl = (InterfaceB)obj;
    interfaceBImpl.doSomeThing();
  }
}
```

Because Interface Injection is aggressive and will be combined with specific interface, so it's not popular.

### Setter Injection

``` java
public class ClassA {
  private ClassB classB;
  public void setClassB(ClassB classB) {
    this.classB = classB;
  }
}
```

Suitable for small amount of properties. 

### Constructor Injection

``` java
public class ClassA {
  private ClassB classB;
  public ClassA(ClassB classB) {
    this.classB = classB;
  }
}
```

Suitable for large amount of properties.

### Annotation for DI

- @Autowired - default wiring by type
  - @Qualifier("") -  if have several same type Bean, wiring by name
  - @Primary - if have several same type Bean, wiring primary Bean first
- @Resource(name="") - default by name

## IoC Container

- BeanFactory - Spec of IoC container, lazy load
  - ApplicationContext - support AOP plugin, resource access; instant load
  - verse FactoryBean: a interface for customize a bean factory

# Bean

## Annotation 

- @Component usually used on a Class
  - @Controller, @Service, @Repository
- @Configuration Class and @Bean method, usually for third party library
- @Scope("") for how Spring create a Bean
  - signleton - default
  - prototype
  - request
  - session
  - global session

# Spring database connection

- JDBC - Java DataBase Connection
  - JPA - Java Persistence API - spec for ORM framwork
    - JPA implementation: Hibernate, Spring Data JPA (also use Hibernate)

# AOP

## Concept

- Aspect 切面 - basic unit of AOP, like Class of OOP
- Advice 通知 - a group of code that Aspect add to specific Join Point
  - Before Advice - Runs before Join Point, cannot prevent execution of Join Point unless throws an exception
  - After runing / After throwing Advice
  - After (finally) Advice - Runs anyway after Join Point exits
  - Around Advice - Runs before and after Join Point executes
- Join Point 连接点 - represent an execution of method 
- Point Cut 切点 - rule for matching Join Point with Advice
  - @Pointcut("execution(public * *(..))") 任意公共方法
  - @Pointcut("within(package.class.*") class包内任意方法
  - @Pointcut("within(package.class..*") class及子包内任意方法
  - @Pointcut("Bean(*Service)") 以Service结尾的任意Bean
- Introduction 引入 - extra method or parameter that introduced to Target
- Target / Advised Object - An object that been weaving with advice
- AOP proxy - An object created by Spring to implement the AOP contracts
- Weaving 织入 - linking Aspects with other objects and create an Advised Object
  - Weaving at runtime - Spring AOP
  - Weaving at compile time - AspectJ compiler
  - Weaving at load time

## @Transactional - TODO

Close auto-commit in DB connection

Will not effect on `non-public` function and inner class call.

Don't let I/O connection execute inside transactional, may cause the connection been hold for long time.

# Spring MVC

## DispatcherServlet

![img](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/spring-dispatcher-servlet.png)