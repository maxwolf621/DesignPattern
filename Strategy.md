# Strategy

###### tags: `Design Pattern`

>Strategy
>: A class behavior or its algorithm can be changed to expect one at run-time

![](https://i.imgur.com/QskFpjB.png)
> Context
> :  something encapsulates some kind of state.
```python=
class concreteStragies1:
    def algorithm(self,'''parameters'''):
        #...
class concreteStragies2:
    def algorithm(self,'''parameters'''):
        #....

```



The Strategy pattern suggests: 
1. Encapsulating an algorithm/behavior in a class hierarchy (implement the interface)
2. Having clients of that algorithm holds a pointer to the base class of that hierarchy (Class Context)
3. Delegating all requests for the algorithm to that "anonymous" contained object. (ExecuteStraegy(..) method in Context)


## Example of doing + or -  for arithmetic ...

#### interface Strategy
```java
public interface Strategy{
 public int algorithm()
}
```

#### Create different *Concrete*Strategies 

for different behaviors/algorithms we got these
```java
/* assume we got for now for two different behaviors ..*/

public class doAdd implements Strategy{
  @Override
  public int algorithm(int a ,int b){
    //do something
  }
}

public class doSubstract implements Strategy{
  @Override
  public int algorithm(int a, int b){
    //do something
  }
}
```

#### Context Class 
- this allows us use difference strategies(behaviors) at run-time
    > it encapsulates the sate of concrete strategies
```java=
public class Context{
    private Strategy strategy;

    public Context(Strategy strategy){
    this.strategy = strategy;
    }
    // To Execute The Contrete Strategies
    public int ExecuteStrategy(int a, int b){
        return strategy.algorithm(int a, int b);
    }
}
```

Run the Strategy Pattern
```java=
public class StrategyPattern{
  public static void main(String[] args){
    // Create A instance of Context
    // Constructor of Context creates instance of Strategy
    Context add = new Context(new doAdd());
    Context substract = new Context(new doSubstract()));
    
    /* we can have different algorithms in run-time
        such as add , substract ... 
    */
    int a = 5;
    int b = 1;
    // Execute the strategy for adding
    add.ExecuteStrategy(a,b); 
    substract.ExecuteStrategy(a,b);
  }
}
```
