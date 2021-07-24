# Reference
- [IntroductionCN](https://www.cnblogs.com/HOsystem/p/14660488.html#%E5%88%9B%E5%BB%BA%E5%9E%8B%E6%A8%A1%E5%BC%8F)  
- [CppDesignPattern](https://medium.com/must-know-computer-science/basic-design-patterns-in-c-39bd3d477a5c)  
- [IntroductionTW](https://ianjustin39.github.io/ianlife/design-pattern/singleton-pattern/)

# Basic
* [UML Diagram](UML_Diagram.md)
* [Dependency Injection](DependencyInjection.md)

# Principle
- [Principle](Principle.md)
  - SRP(Single Responsibilities Principle)
  - ISP(Interface Segregation Principle)
  - DIP(Dependence Inversion Principle)
  - LSK(Liskov Substitution Principle)
  - OCP(Open Close Principle)
  - CARP	Composite/Aggregate Reuse Principle
  - LKP	Least Knowledge Principle/ DP (Demeter Principle)

# Creational Pattern
Creational patterns provide various object creation mechanisms, which increase flexibility and reuse of existing code.
- Creational design patterns are concerned with the way of creating objects. 

With this pattern we can avoid such way to create a object  
```Java
ObjectX s1 = new ObjectX();  
```

* [Factory](C_Factory.md)
* [Abstract Factory](C_AbstractFactory.md) 
* [Singleton](C_Singleton.md)
* [Builder](C_Builder.md)
* [Prototype](C_Prototype.md)

> keyword for creational pattern : `Create`

# Structural Pattern

Structural design patterns are concerned with how classes and objects can be composed, to form larger structures.  
- These patterns focus on, how the classes inherit from each other and how they are composed from other classes.

> keyword : `Merge` `Combine` 

* [Adapter](S_Adapter.md) 
* [Bridge](S_Bridge.md) 
* [Fascade](S_Fascade.md)
* [Composite](B_Composite.md)
* [Flyweight](S_Flyweight.md)
* [Proxy](S_Proxy.md)
* [Decoration](S_Decoration.md)

# Behavioral Pattern
- In these design patterns, the interaction between the objects should be in such a way that they can easily talk to each other and still should be loosely coupled.
  > That means the implementation and the client should be loosely coupled in order to avoid hard coding and dependencies.


* [Iterator](B_Iterator.md)  
  > ![image](https://user-images.githubusercontent.com/68631186/126871777-72aa072f-acbc-4860-9155-e89399e55d1d.png)
* [Command](B_Command.md)  
* [Memento](B_Memonto.md)  
* [Strategy](B_Strategy.md)
  > ![](https://camo.githubusercontent.com/10a9e43ddadf8f1f96bed1a2f5609eec70e6b30654317498574791c8926c4ead/68747470733a2f2f692e696d6775722e636f6d2f51736b46706a422e706e67)
* [Template](B_Template.md)
* [Observer](B_Observer.md)
* [State](B_State.md)
* [Chain of Responsibility](B_ChainOfResponsibility.md)
* [Interpreter](B_Iterpreter.md)
* [Mediator](B_Mediator.md)
* [Null Object](B_NullObject.md)
* [Visitor](B_Visitor.md)

