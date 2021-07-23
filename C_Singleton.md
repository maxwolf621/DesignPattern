###### tags: `Design Pattern`
# Singleton
[TOC]
   
**The Singleton Pattern ensures a class has only one instance, and provides a global point of access to it.**
> pattern UML  
> ![](https://i.imgur.com/ZhgkHCD.png)  

Examples where singletons are useful are:
- thread pools
- caches
- dialog boxes
- objects that handle preferences and registry settings
- objects used for logging
- objects that act as device drivers to devices like printers and graphics cards.

In fact, for many of these types of objects, if we were to instantiate more than one we’d run into all sorts of problems like incorrect program behavior, overuse of resources, or inconsistent results.  
By making use of the Singleton one can assure that every object in an application is making use of the same global resource.


## Lazy Initialization 
The singleton is similar to global variables but without the downside of getting created at program start like global variables. 
Instead, ***the singleton can be created only when it is needed, which can avoid time consuming initialization.***

> if we never need the instance, it never gets created. This is lazy instantiation.

## Responsibilities 

1. Managing its one instance (and providing global access) 
2. It is also responsible for whatever its main role is intended. 

## `synchronized` singleton pattern
- `synchronized` 以免共用存取時，發生資料的競速（Race condition）問題。

a singleton pattern without **synchronized**
```java
/**
  * @apinote
  *   It is not thread safe
  *   <strong> this will cause the problem while applying for multiple threading </strong>
  *   <strong> A singleton should not be inherited from because its constructor should remain private. </strong>
  */
public class singleton{

    private static singleton unique;
    
    /**
      * @Description
      *   Create static singleton unique
      *   if {@code unique} is {@code null}
      *   then create one 
      *   else
      *   @return {@code unique}
      */
    pubic static singleton getsingleton()
    {
        if(unique == null){
            unique = new Singleton();
        }
        return unique;
    }
    
    //..
}
```
- In this case, Singleton is instantiated through its private constructor and assigned to to `uniqueInstance`. 
- If uniqueInstance wasn’t null, then it was previously created and is therefore returned. Because the `getInstance()` is a `static` method, it allows access from anywhere in the code using `Singleton::getInstance()`. This is just as easy as accessing a global variable but with the advantage of lazy instantiaion(使用到在創見Instance).


For Thread Safe we just add `synchronized` at ...
```java
public static synchronized singleton getsingleton(){
    //...
}
```
- Using `synchronize`, is expensive and the synchronization is only relevant the first time when there was no instance created yet. 
- ***Once we've set the `unique` to an instance of singleton, we have no further need to synchronize this method.***

## `new` a private instance of singleton

If your application always creates and uses an instance of Singleton or the overhead of creation and run-time aspects of the Singleton are not onerous then
```java
public class Singleton {
    // default constructor does notrhing
    private Singleton() {}
    
    // Guaranteed thread safe
    //  and also get away of affecting of synchronization  
    private static Singleton unique = new Singleton();

    // now it just reutrns the unique-instance
    public static Singleton getInstance() {
        return unique;
    }
    //....
}
```

### Double-checked Locking
This also can reduce `synchronization` costs 

It would first check if an instance of singleton is created, and if not (`== null`) then using `synchronized` to create one. 
This means using `synchronized` at the first time for creating a unique instance of singleton
```java
/**
  * @apinote
  *   Only creating <strong > synchronized </strong> Singleton <strong> one only time </strong>
  */
public class Singleton {
    private volatile static Singleton unique;
    private Singleton() {}
    /**
      * <p> using <pre> synhronized </pre> only once
      *     while creating a instance
      */
    public static Singleton getInstance() {
        if (unique == null) {
            synchronized (Singleton.class) {
                if (unique  == null) {
                    unique  = new Singleton();
                }
            }
        }
        return unique;
    }
}
```


