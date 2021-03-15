==In Strategy pattern, a class behavior or its algorithm can be changed (you can change other algorithm  at `run time`.==




For example 
Create A Strategy for arithmetic ...
For now we just need the arithemtic for Add, Substract ..

First we create interface A Class
```java
public interface Strategy{
 public int algorithm()
}
```

Create Concrte Classes (To Do add, To Do substract) implementing Strategy for different behaviours/algorithms
```java
public class doAdd implements Strategy{
  @Override
  public int algorithm(/* .. */){
    //.....
  }
}

public class doSubstract implements Strategy{
  @Override
  public int algorithm(/*...*/){
    //.....
  }
}
```




Create Context Class 
to access the instance of strategy and use the difference strategies
```java
public class Context{
  private Strategy strategy;
  
  // I need a method access private member
 public Context(Strategy strategy){
   this.strategy = strategy;
 }
 
 // To Execute The Strategies
 public int executeStrategy( /*...*/){
  return strategy.algorithm(/*...*/);
 }
}

public class StrategyPattern{
  public static void main(String[] args){
    // Create A instance of Context
    // Constructor of Context creates instance of Strategy
    Context add = new Context(new doAdd());
    Context add = new Context(new doAdd());
    
    /* we can have different algorithms in run-time
        such as add , substract ... 
    */
    
    // Execute the strategy for adding
    add.executeStrategy( /*...*/); 
    substract.executeStrategy(/*...*/);
  }
}
```


The Strategy pattern suggests: 
1. Encapsulating an algorithm/behaviour in a class hierarchy (class doAdd, class doSubstract ...)
2. Having clients of that algorithm hold a pointer to the base class of that hierarchy (Class Context)
3. Delegating all requests for the algorithm to that "anonymous" contained object. (executeStraegy() )
