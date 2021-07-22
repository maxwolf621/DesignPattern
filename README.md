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

* [Factory](Factory.md)
* [Abstract Factory](AbstractFactory.md) 
* [Singleton](Singleton.md)
* [Builder](Builder.md)
* [Prototype](Prototype.md)

> keyword for creational pattern : `Create`

# Structural Pattern

Structural design patterns are concerned with how classes and objects can be composed, to form larger structures.  
- These patterns focus on, how the classes inherit from each other and how they are composed from other classes.

> keyword : `Merge` `Combine` 

* [Adapter](adapter.md) 
* [Bridge](Bridge.md) 
* [Fascade](Fascade.md)
* [Composite](Iterator_and_Composite.md)
* [Flyweight](Flyweight.md)
* [Proxy](Proxy.md)
* [Decoration](Decoration.md)

# Behavioral Pattern
- In these design patterns, the interaction between the objects should be in such a way that they can easily talk to each other and still should be loosely coupled.
  > That means the implementation and the client should be loosely coupled in order to avoid hard coding and dependencies.


* [Iterator](Iterator_and_Composite.md)  
* [Command](Command.md)  
* [Memento](Memonto.md)  
* [Strategy](Strategy.md)
* [Template](Template.md)
* [Observer](Observer.md)
* [State](State.md)
* [Chain of Responsibility](Chain_of_Responsibility.md)
* [Interpreter](Iterpreter.md)
* [Mediator](Mediator.md)
* [Null Object](Null_Object.md)
* [Visitor](Visitor.md)

