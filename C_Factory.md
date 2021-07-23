###### tags: `Design Pattern`
# Factory  
[Source Code](https://fjp.at/design-patterns/factory)  
[Another Example](https://github.com/iluwatar/java-design-patterns/tree/master/factory) 
> Factory-Muster  
>![](https://i.imgur.com/kKFCAa7.png)  
- Factory Pattern provides an interface for creating objects in a super-class, but allows sub-classes to alter the type of objects that will be created.  

## Factory Pattern
```java
/**
  * <p> A abstract class that 
  *   contains the implementations
  *   for all of the methods to
  *   <strong> manipulate products </strong>
  *   <strong> except for the factory method </strong>
  * </p>
  */
public abstract Creator{
    private Product product;

    /**
     * Factory methods for Concrete Creator
     */
    abstract Product createProduct(/* parameters */)
    
    /**
     * Methods to manipulate the products
     */
    public Operation_1(/* parameters */){
        /**
          * <strong> we can use Factory methods 
          *          to do some operation </strong >
          */
        product = createProduct(/*..*/);
    }
}

/**
  * <p> concrete Creators </p>
  */
public concreteCreatorForProduct1 extends Creator{
    private Product product;
    
    //..
    
    /**
      * return a certain concrete product
      */
    public Product createProduct(/*parameters*/)
    {
        //.....
        return product
    }
}

public concreteCreatorForProduct2 extends Creator{/*...*/}
//... other Creators ...

/**
  * <p> The Specification of Product
  *     e.g. methods can be how Product made of ...etc </p>
  * <p> Product can be car, pizza ...etc </p> 
  */
public abstract Product{/*...*/}

/**
  * <p> Concete Product will be used 
  *     in concrete Creators </p>
  * <p> if Product is car then concrete product can be 
  *     car from different country bmw, honda, ford ... </p> 
  * <p> the methods should be specifically how this certain concrete product being made</p>
  */
public concreteProduct1 extends Product{/*...*/}
public concreteProduct2 extends Product{/*...*/}
// ... other concreteProducts
```


## Abstract Factory

> Pattern  
>![](https://i.imgur.com/0DMqDUd.png)    
>![](https://i.imgur.com/gWUWtKV.png)    
- Abstract Factory pattern lets us produce families of related objects without specifying their concrete classes.   
