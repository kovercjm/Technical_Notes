# Classification

- Creational Pattern 创建型模式
  - Singleton 单例
  - Factory Method 工厂方法
  - Abstract Factory 抽象工厂
  - Builder 建造者
  - Prototype 原型
- Structural Pattern 结构型
  - Adapter 适配器
  - Bridge 桥接
  - Composite 组合
  - Decorator 装饰器
  - Facade 外观
  - Flyweight 享元
  - Proxy 代理
- Behavior Patterns 行为型
  - Chain of responsibility 责任链
  - Command 命令
  - Interpreter 解释器
  - Iterator 迭代子
  - Mediator 中介者
  - Memento 备忘录
  - Observer 观察者
  - State 状态
  - Strategy 策略
  - Template Method 模板方法
  - Visitor 访问者

# SOLID concept

- The Single-responsibility principle
  - A class should have one and only one reason to change, meaning that a class should have only one job.
- The Open–closed principle
  - Objects or entities should be open for extension but closed for modification.
- The Liskov substitution principle
  - Every subclass or derived class should be substitutable for their base or parent class.
- The Interface segregation principle
  - Many client-specific interfaces are better than one general-purpose interface.
- The Dependency inversion principle
  - Entities must depend on abstractions, not on concretions.

# Demeter principle

- Each unit should have only limited knowledge about other units: only units “closely” related to the current unit.
- Each unit should only talk to its friends; don’t talk to strangers.
- Only talk to your immediate friends.

# Composite/Aggregate Reuse principle

Favor delegation over inheritance as a reuse mechanism

# Pattern Details

## Singleton

Ensure a class has only one instance, and provide a global point of access to it.

- Implementation:
  - Set default constructor as private
  - Have a public static function returns a private 'singleton' object
  - If 'singleton' object not existed, calls private constructor
- Example:
  - Counter, Thread Pool or Mac Activity Monitor
- Advantage:
  - Easy control of the sharing object
- Problem:
  - Violate Single-responsibility Principle, be factory and product at the same time
  - Doesn't support changing
  - Cannot be reflected
  - Lazy Instantiation may not be concurrent-safe

## Factory Method and Abstract Factory

Define an interface for creating a single object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

Simple Factory is the same except all types of objects are instantiated by itself except of subclasses.

Abstract Factory has a higher-level Factory for deside which Factory is used to create instance.

- Example:
  - Bean creation in Spring IoC (a map for singleton object)
- Advantage:
  - Obey Single-responsibility and Open-close Principle
  - Decoupling the system
- Problem:
  - Complicated coding for creating many subclasses

## Builder

Separate the construction of a complex object from its representation, allowing the same construction process to create various representations.

- Example:
  - Java StringBuilder
- Applied Scenerio:
  - Generate objects that have complex inner structure
  - Generate objects that have inner dependency or order of generation
- Advantage:
  - Obey Single-responsibility and Open-close Principle
- Problem:
  - Complicated coding for creating many subclasses
  - Hard to create objects that have large inner difference

## Prototype

Specify the kinds of objects to create using a prototypical instance, and create new objects from the 'skeleton' of an existing object, thus boosting performance and keeping memory footprints to a minimum.

- Problem:
  - Clone can be copy or deepcopy

## Adapter

Convert the interface of a class into another interface clients expect. An adapter lets classes work together that could not otherwise because of incompatible interfaces. The enterprise integration pattern equivalent is the translator.

- Example:
  - Each DB engine has JDBC driver (adapter) for integration with JDBC interface
- Advantage:
  - Obey Single-responsibility and Open-close Principle
- Problem:
  - Complicated coding

## Bridge

Decouple an abstraction from its implementation allowing the two to vary independently.

- Advantage:
  - Obey Single-responsibility and Open-close Principle
  - Have Implementation as a reference of Abstraction; composition instead of inheritance
- Problem:
  - Complicated coding
  - Hard to identify two dimension of individually changing class

## Composite

Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.

- Advantage:
  - Obey Open-close Principle

## Decorator

Attach additional responsibilities to an object dynamically keeping the same interface. Decorators provide a flexible alternative to subclassing for extending functionality.

- Advantage:
  - Obey Single-responsibility Principle
  - Expand an object's ability without inheritance
- Problem:
  - Decorator is decorated in stack, hard to delete or run without order

## Facade

Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.

- Problem:
  - Changing of subsystem may need to modify facade class; may violate Open-close Principle

## Flyweight

Use sharing to support large numbers of similar objects efficiently.

- Advantage:
  - Save memory by keep only one copy of similar object, decided by flyweight factory
- Problem:
  - Processing speed may be sacrificed by retrieving status of flyweight objects

## Proxy

Provide a surrogate or placeholder for another object to control access to it.

- Type: Remote, Virtual (lazy-loading), Copy-on-Write, Protect or Access, Cache, Firewall, Synchronization, Smart Reference
- Example:
  - Dynamic Proxy: Spring AOP
- Advantage:
  - Obey Open-close Principle
  - Often contains life-circle management of service object
- Problem:
  - Complicated coding and delay of response

## Chain of responsibility

Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handles it.

- Advantage:
  - Obey Single-responsibility and Open-close Principle
- Problem:
  - Some request may not be handled

## Command

Encapsulate a request as an object, thereby allowing for the parameterization of clients with different requests, and the queuing or logging of requests. It also allows for the support of undoable operations.

- Advantage:
  - Obey Single-responsibility and Open-close Principle
  - Can have 'cancel', 'recover', 'lazy-process' function, or have combined Marco command
- Problem:
  - Complicated coding

## Interpreter

Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language.

## Iterator

Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

- Advantage:
  - Obey Single-responsibility and Open-close Principle
  - Can have concurrency iterators and flexible control of iterator
- Problem:
  - Not for simple set of data

## Mediator

Define an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it allows their interaction to vary independently.

- Advantage:
  - Obey Single-responsibility and Open-close Principle
  - Can loose coupling and good for reusing
- Problem:
  - May become God Object

## Memento

Without violating encapsulation, capture and externalize an object's internal state allowing the object to be restored to this state later.

- Only Originator has full access to its Memento, Caretakers stores the stragey of restoring

## Observer

Define a one-to-many dependency between objects where a state change in one object results in all its dependents being notified and updated automatically.

- Advantage:
  - Obey Open-close Principle

## State

Allow an object to alter its behavior when its internal state changes. The object will appear to change its class.

- Advantage:
  - Obey Single-responsibility and Open-close Principle
  - Can simplify finite-state machine

## Strategy

Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

- Advantage:
  - Obey Open-close Principle

## Template Method

Define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

- Advantage:
  - Can rewrite certain part with small influction on other part of an algorithm
- Problem:
  - Rewrite in subclass may violate Liskov substitution principle

## Visitor

Represent an operation to be performed on the elements of an object structure. Visitor lets a new operation be defined without changing the classes of the elements on which it operates.

- Advantage:
  - Obey Single-responsibility and Open-close Principle

# Reference

- [Design Patterns Catalog](https://refactoringguru.cn/design-patterns/catalog)

