# Template Method

- To Prevent from duplicate codes
- The Template Method defines the steps of an algorithm and allows subclasses to provide the implementation for one or more steps.

![](https://i.imgur.com/qhYbfmb.png)
```java=
abstract class AbstractClass {
      /**
       * It's declared final to prevent subclaasses 
       *   from reworking the sequence of steps in the algorithm
       */
      final void templateMethod()
      {
        primitiveOperation2();
        primitiveOperation1();
        concreteOperation();   // the common parts of subclasses
      }

     /**
      * The Followings Must be implemented by concrete subclasses
      */
     abstract void primitiveOperation1();
     abstract void primitiveOperation2();

     /** 
      You can also define a concrete operations
        defined in the abstract class
      It may
        used by in the template method or 
        used by by subclasses
     */
     void concreteOperation()
     {
         //implementation here
     }


     /** 
      A concrete method that do nothing or something
          the sub-classes can override this method 
          but dosen't have to
      */
     void hook()
     {
         //...
     }
}
```


> A hook is a method that is declared in the
abstract class, but only given an empty
or default implementation.
>> This gives sub-classes the ability to “hook into” the
algorithm at various points


## Template Method Using with the hook

```java=
public abstract class CaffeineBeverage{
    final void prepareRecipe() {
        boilWater();
        brew(); // implemented by subclasses
        pourInCup();
        
        // using hook makes code more flexible
        if (customerWantsCondiments()) {
            // implemented by subclasses
            addCondiments();  
        }
    }
    abstract void brew();
    abstract void addCondiments();
    
    /* 
     * Concrete operation 
     */
    void boilWater() {
    System.out.println(“Boiling water”);
    }
    void pourInCup() {
    System.out.println(“Pouring into cup”);
    }
    
    /* A hook */
    boolean customerWantsCondiments() {
        return true;
    }
}

/* sub-class */
publicclass Coffee extends CaffeineBeverage {
    
    /* let's implement abstract functions from the base */
    public void brew() {
        System.out.println(“Dripping Coffee through filter”);
    }
    public void addCondiments() {
        System.out.println(“Adding Sugar and Milk”);
    }
    
    /* implement the hook */
    public boolean customerWantsCondiments() {
        String answer = getUserInput();
        if (answer.toLowerCase().startsWith(“y”)) {
            return true;
        } else {
            return false;
        }
    }
    private String getUserInput() {
        String answer = null;
        System.out.print("Would you like milk and sugar with your coffee (y/n)?");
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        try {
            answer = in.readLine();
        } catch (IOException ioe) {
            System.err.println(“IO error trying to read your answer”);
        }
        if (answer == null) {return "no"; }
        
        return answer;
         
    }
}

/* subclass with tea */
publicclass Tea extends CaffeineBeverage
{
    public void brew() {
        System.out.println("Steeping the tea");
    }
    public void addCondiments() {
        System.out.println("Adding Lemon");
    }
    
    //....
}
public class BeverageTestDrive {
public static void main(String[] args) {
    Coffee coffee = new Coffee();
    Tea tea = new Tea();
    System.out.println("\nMaking Coffee...");
    coffee.prepareRecipe();
     System.out.println("\nMaking Tea...");
    tea.prepareRecipe();
 }
}

```


## Hollywood Principle 


The Hollywood principle gives us a way to prevent dependency rot.

> Dependency rot 
> : it happens when you have high-level components depending on low-level components
depending on high-level components depending on sideways
components depending on low-level components, and so on.


With the Hollywood Principle, we allow low-level components
to hook themselves into a system, but the high-level
components determine when they are needed.

In other words, the high-level components give the low-level
components a treatment of `don’t call us, we’ll call you` 

![](https://i.imgur.com/IKZGoPi.png)


## Template Method and Hollywood Principle

1. Class CoffeineBeverage has control over the algorithm for the recipe and calls on the sub-classes only when they are needed
2. Class Tee and Class Coffee call the abstract directly without being called first

![](https://i.imgur.com/duKNSFW.png)
