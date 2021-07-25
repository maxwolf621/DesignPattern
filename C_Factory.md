###### tags: `Design Pattern`
# Factory  
[Source Code](https://fjp.at/design-patterns/factory)   
[Example from iluwater](https://github.com/iluwatar/java-design-patterns/tree/master/factory)   
[Example from cyc](https://github.com/CyC2018/CS-Notes/blob/master/notes/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%20-%20%E5%B7%A5%E5%8E%82%E6%96%B9%E6%B3%95.md)

> Factory-Muster  
>![](https://i.imgur.com/kKFCAa7.png)  
- Factory Pattern provides an interface for creating objects in a super-class, but allows sub-classes to alter the type of objects that will be created.  

```java
/**
  * <p> Factory </p>
  * <strong> manipulate products </strong>
  * <strong> except for the factory method </strong>
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
    public OperationForProduct(/* parameters */){
        /**
          * <strong> we can use Factory method
          *          {@code createProduct}
          *          to do some operations </strong >
          */
        product = createProduct(/*..*/);
    }
}

/**
  * <p> concrete Creators(Factory) </p>
  */
public concreteCreatorForProduct1 extends Creator{
    private Product product;
    
    //..
    
    /**
      * @return a certain product
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
  * <p> For example if Product is car 
  *     then concrete product can be 
  *     car from different country bmw, honda, ford ...etc </p> 
  * <p> the methods in concrete product
  *     should be specifically
  *     how this certain concrete product being made</p>
  */
public concreteProduct1 extends Product{/*...*/}
public concreteProduct2 extends Product{/*...*/}
// ... other concreteProducts
```
