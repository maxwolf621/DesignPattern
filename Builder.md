# Builder 

Think the pattern as the customized demand.

For example
```
BigMac meal : BigMac + cola
Chicken Burger meal : Chicken Burger + pesi
```
> The guests can choose different meal depending on their preference

And also we do not want to have a complex constructor member or 
one that would need many arguments.

So by using Builder Pattern, we define an instance for creating an object 
but letting subclasses decide which class to instantiate and 
refer to the newly created object through a common interface.


Common interface class Item
```java
public interface Item{
  public string name();
  pbulic Packing packing();
  public float price();
)
```

```java
/* To PACK cola or burger */
public interface Packing {
   public String pack();
}
// To pack the burger
public class Wrapper implements Packing {

   @Override
   public String pack() {
      return "Wrapper";
   }
}
// To pack the cola
public class Bottle implements Packing {

   @Override
   public String pack() {
      return "Bottle";
   }
}
```

General 
```java
public abstract class Burger implements Item{
  @override
   @Override
   public Packing packing() {
      return new Wrapper();
   }
   @Override
   public abstract float price();
}
public abstract class ColdDrink implements Item {
	@Override
	public Packing packing() {
       return new Bottle();
	}
	@Override
	public abstract float price();
}
```


Burger flavour : BigMacBurger, ChickenBurger
Coldrink : Pepsi, Coke
```java
public class BigMac extends Burger{
  @Override
  public float price(){
    retrun 79.0f;
  }
  @Override
  public String name(){
    return "Big Mac";
  }
}

public class ChickenBurger extends Burger{
  @Override
  public float price(){
    retrun 60.0f;
  }
  @Override
  public String name(){
    return "Chicken Burger";
  }
}

public class Pepsi extends ColdDrink {
   @Override
   public float price() {
      return 35.0f;
   }
   @Override
   public String name() {
      return "Pepsi";
   }
}

public class Coke extends ColdDrink {
   @Override
   public float price() {
      return 30.0f;
   }
   @Override
   public String name() {
      return "Coke";
   }
}
```

Create A class to manipulate objecrs from class Item 
```
import java.util.ArrayList;
import java.util.List;

public class Meal {
   private List<Item> items = new ArrayList<Item>();	

   // choose the burger and colddrink where guest needs
   public void addItem(Item item){
      items.add(item);
   }

   public float getCost(){
      float cost = 0.0f;
      
      for (Item item : items) {
         cost += item.price();
      }		
      return cost;
   }

 
   public void showItems(){
      for (Item item : items) {
         System.out.print("Item : " + item.name());
         System.out.print(", Packing : " + item.packing().pack());
         System.out.println(", Price : " + item.price());
      }		
   }	
}
```


Builder To create items/products using just one instance 
(To prepare a customized Product to guest)
```java
public class MealBuilder {
   public Meal createBigMacMeal (){
      Meal meal = new Meal();
      meal.addItem(new BigMac());
      meal.addItem(new Coke());
      return meal;
   }   

   public Meal createChickenMeal (){
      Meal meal = new Meal();
      meal.addItem(new ChickenBurger());
      meal.addItem(new Pepsi());
      return meal;
   }
}
```
> Although each meal has lots of items but 

```java 
public builder 
