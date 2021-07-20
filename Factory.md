###### tags: `Design Pattern`
# Factory  
[REF](https://fjp.at/design-patterns/factory)  


> Factory-Muster  
>![](https://i.imgur.com/kKFCAa7.png)  
- Factory Pattern provides an interface for creating objects in a super-class, but allows sub-classes to alter the type of objects that will be created.  

> Abstract Factory  
>![](https://i.imgur.com/0DMqDUd.png)  
>![](https://i.imgur.com/gWUWtKV.png)  
- Abstract Factory pattern lets us produce families of related objects without specifying their concrete classes.  

## Factory Pattern
```java
/**
  * A abstract class that 
  *   contains the implementations
  *   for all of the methods to
  *   <strong> manipulate products </strong>
  *   <strong> except for the factory method </strong>
  */
public abstract Creator{
    private Product product;

    /**
     * factory method
     */
    abstract Product createProduct(/* parameters */)
    
    /**
     * Methods Manipute products
     */
    public Operations_1(){
        product = createProduct(/*..*/);
        //...
    }
    
    public 
    
}
// concrete Creators
public Creator_1 extends Creator{
    private Product product
    //..
    public Product createProduct(/*parameters*/)
    {
        product_1 p1;
        //.....
    }
}
public Creator_2 extends Creator{
    //...
}
//... so on ...
```
```java=
public abstract Product{
    //....
}
// Concete Product will be used in concrete Creators
public product_1 extends Product{
    //...
}
```
