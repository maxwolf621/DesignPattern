
###### tags: `Design Pattern`
# Composite and Iterator

> Iterator-Muster  
![](https://i.imgur.com/murQQP1.png)  
![image](https://user-images.githubusercontent.com/68631186/126357563-ae3639d8-19ba-434d-b515-5372f6b50341.png)  

## Example without Iterator pattern

To merge two menus  
>For example  
>![](https://i.imgur.com/WwWcYtK.png)  

```java
/**
  *<p> If we have two menus as the following  </p>
  *	<li> DinerMenu </li>
  *	<li> PancakeHouseMenu </li>
  *<p> Two menus have different structures </p>
  *<p> PacakeHouseMenu stores its items via {@code ArrayList<MenuItem>} </p>
  *<p> DinerMenu stores its items via {@code MenuItem[]} </p>
  */
  
public class MenuItem{
    String name;
    String description;
    boolean vegetarian;
    double price;
    
    public MenuItem(String name, String description,
                    boolean vegetarian, double price){
                        /* initialize ... */
    }
    
    // other methods ...
}

public class PancakeHouseMenu{
    ArrayList<MenuItem> menuItems;
    
    public PancakeHouse(){
        menuItems = new ArrayList<MenuItem>();
        addItem(/* name, description, price */);
        //....
    }
    
    /**
      * @Description
      *    Add the item in the menu
      */
    public void addItem(String name, String description,
                boolean vegetarian , double price)
                {
                    MenuItem menuItem = new MenuItem(/*..*/);
                    menItems.add(menuItem);
                }
}

public class DinerMenu{
    static final int MAX = 6;
    
    int items = 0;
    MenuItem[] menuItems;
    
    public DinerMenu(){
    	menuItems = new MenuItem[MAX];
	
	addItem(/*...*/);
	addItem(/*...*/);
	//.. add rest 
    }
    
    public void addItem(String name, String description,
                boolean vegetarian, double price){
                //...
    }
    public MenuItem[] getMenuItems(){
        return menuItems;
    }
    //...
}
```
- Both menus use different way to contain their information(e.g. description, name ... etc). 
    > One uses ArrayList and the other uses Array.

Now A waitress handle two menus sorta like this
```java
public class waitress{
    PancakeHouseMenu pancakeHouseMenu = new PancakeHOuseMenu();
    ArrayList<MenuItem> breakfastItems = pancakeHouseMenu.getMenuItems();
    
    DinerMenu dinerMenu = new DinerMenu();
    MenuItem[] LunchItems = dinerMenu.getMenuItems();
    
    //For PancakeHouseMenu
    for(int i = 0 ; i < breakfastItems.size() ; ++i)
    {
        MenuItem menuItem = breakfastItems.get(i);
	//...
    }
    
    //For dinerMenu 
    for(int i = 0; i < lunchItems.lengtho ; ++i){
        MenuItem menuItem = lunchItem[i];
	//...
    }
}
```
- here comes to the problem.
    > The Waitress need to have two different loop to iterate over the two different kinds of menus.  
    > Also if we added a third menu, we'd have yet another loop to iterate.  
    > **This definitely gives us (the Waitress) hard to maintain and extend (Against Design Pattern Rule)**

To solve such problem we can use `iterator pattern`
> Iterator  
> provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.  
> ![image](https://user-images.githubusercontent.com/68631186/126334313-5ebfecb9-4e95-4dcf-a7de-151dc5b5ceca.png)

To have less coupled we can change the UML like this
![image](https://user-images.githubusercontent.com/68631186/126357357-ce94e05b-9072-4fb8-93aa-0fd896663341.png)


[SourceCode](https://github.com/bethrobson/Head-First-Design-Patterns/tree/master/src/headfirst/designpatterns/iterator/dinermerger)

```java
/**
  * Adapter Pattern
  */
public interface Iterator{
    boolean hasNext()
    MenuItem next()
}

public class ArrayListIterator implements Iterator{
	ArrayList<MenuItem> items;
	int position = 0;

	publkic ArrayListIterator(ArrayList<MenuItem> items){
		this.items = items;
	}
	
	/**
	  * @Description
	  *	Go next element 
	  */
	public MenuItem next(){
		return items.get(position++);
	}
	
	public boolean hasNext(){
		return items.size() > position;
	}
}
public class ArrayIterator implements Iterator{
	MenuItem[] items;
    	int position = 0;
	
	public ArrayIterator(MenuItem[] items) {
		this.items = items;
	}
	public MenuItem next() {
		return items.get(position++);
	}

	public boolean hasNext() {
		/*
		if (position >= items.length || items[position] == null) {
			return false;
		} else {
			return true;
		}
		*/
	
		return items.length > position;
	}
}


/**
  * <p> Menu that creats Iterator </p>
  */
public interface Menu{
	Iterator createIterator();
}

/**
  * <p> Implatement our PackeHouseMenu and DinerMenuy 
  *	with Menu for create an Iterator </p>
  */
public class PancakeHouseMenu implements Menu{
	//..
	
	public Iterator createIterator(){
		return new PancakeIterator(menuItems);
	}
}
public class DinerMenu implements Menu{
	//..
	
	public Iterator createIterator() {
		return new DinerMenuIterator(menuItems);
	}
}
```
-  `MenuItem` Class : Methods to set up each meal's name,description and price
-  `Menu` Interface : Create an Iterator method (so the menus can be used as Iterator)
-  `Iterator` Interface : (adapter pattern) Different structure menus can use the same methods(e.g `next()`, `hasNext()` ... etc)

Now Watiress can use methods 
```java
public class Waitress{
       /**
	 * <p> We can use Menu instead of concreateMenus. </p>
	 *  <strong> Always programming to an interface,
	 *  Not an implementation for Less Decouple </strong>
	 */
	Menu pancakeHouseMenu; // PanckaeHouseMenu pancakeHouseMenu <-- sucks	
	Menu dinerMenu;        // DinerMenu dinerMenu <-- sucks 
    
	public Waitress(Menu pancakeHouseMenu, Menu dinerMenu) {
		this.pancakeHouseMenu = pancakeHouseMenu;
		this.dinerMenu = dinerMenu;
	}
 
	public void printMenu() {
		Iterator pancakeIterator = pancakeHouseMenu.createIterator();
		Iterator dinerIterator = dinerMenu.createIterator();

		System.out.println("MENU\n----\nBREAKFAST");
		printMenu(pancakeIterator);
		System.out.println("\nLUNCH");
		printMenu(dinerIterator);

	}
 
	private void printMenu(Iterator iterator) {
		while (iterator.hasNext()) {
			MenuItem menuItem = iterator.next();
			System.out.print(menuItem.getName() + ", ");
			System.out.print(menuItem.getPrice() + " -- ");
			System.out.println(menuItem.getDescription());
		}
	}
 
	public void printVegetarianMenu() {
		printVegetarianMenu(pancakeHouseMenu.createIterator());
		printVegetarianMenu(dinerMenu.createIterator());
	}
 
	public boolean isItemVegetarian(String name) {
		Iterator breakfastIterator = pancakeHouseMenu.createIterator();
		if (isVegetarian(name, breakfastIterator)) {
			return true;
		}
		Iterator dinnerIterator = dinerMenu.createIterator();
		if (isVegetarian(name, dinnerIterator)) {
			return true;
		}
		return false;
	}


	private void printVegetarianMenu(Iterator iterator) {
		while (iterator.hasNext()) {
			MenuItem menuItem = iterator.next();
			if (menuItem.isVegetarian()) {
				System.out.print(menuItem.getName());
				System.out.println("\t\t" + menuItem.getPrice());
				System.out.println("\t" + menuItem.getDescription());
			}
		}
	}

	private boolean isVegetarian(String name, Iterator iterator) {
		while (iterator.hasNext()) {
			MenuItem menuItem = iterator.next();
			if (menuItem.getName().equals(name)) {
				if (menuItem.isVegetarian()) {
					return true;
				}
			}
		}
		return false;
	}
}
```

## Using `java.util.Iterator` package to resize our Menus

[SourceCode](https://github.com/bethrobson/Head-First-Design-Patterns/blob/master/src/headfirst/designpatterns/iterator/dinermergeri/Waitress.java)  
![](https://i.imgur.com/xTvQYut.png)  



```java
public interface Iterator{
    boolean hasNext()
    MenuItem next()
    void remove() // the remove element method 
}

// we add remove method in our DinerMenuIterator
public class DinerMenuIterator implements Iterator{
	MenuItem[] list ;
	int position = 0;
	//..
    
        /**
	  * @Description
	  * 	remove the first element and shift each element forward
	  * @throws IllegalStateException
	  */
	public void remove() {
		if (position <= 0) {
			throw new IllegalStateException
				("You can't remove an item until you've done at least one next()");
		}
		if (list[position-1] != null) {
			for (int i = position-1; i < (list.length-1); i++) {
				list[i] = list[i+1];
			}
			list[list.length-1] = null;
		}
	}
}
```

Update our Waitress
```java
public class Waitress{
    Menu pancakeHouseMenu;
    Menu dinerMenu;
    
    //..
    
    public void printMenu(){
        /**
          * The following declaration violates the open closed principle
	  */
        Iterator<MenuItem> pancakeIter = pancakeHouseMenu.createIterator();
        Iterator<MenuItem> dinerIter = dinerMneu.createIterator();
        //...
    }
    //..
}
```

To Follow the open close principle 
```java
import java.util.Iterator;
import java.util.ArrayList;
 
public class Waitress {
	Menu pancakeHouseMenu;
	Menu dinerMenu;
 
	public Waitress(Menu pancakeHouseMenu, Menu dinerMenu) {
		this.pancakeHouseMenu = pancakeHouseMenu;
		this.dinerMenu = dinerMenu;
	}
	
	public void printMenu(int withNewConstructs) {
		ArrayList<MenuItem> breakfastItems = ((PancakeHouseMenu) pancakeHouseMenu).getMenuItems();
		
		//pMenu.forEach(m -> printMenuItem(m));
		
		for (MenuItem m : breakfastItems) {
			printMenuItem(m);
		}
		
		MenuItem[] lunchItems = ((DinerMenu) dinerMenu).getMenuItems();
		for (MenuItem m : lunchItems) {
			printMenuItem(m);
		}
	}
	
	public void printMenuItem(MenuItem menuItem) {
		System.out.print(menuItem.getName() + ", ");
		System.out.print(menuItem.getPrice() + " -- ");
		System.out.println(menuItem.getDescription());
	}

 
	public void printMenu() {
		Iterator<MenuItem> pancakeIterator = pancakeHouseMenu.createIterator();
		Iterator<MenuItem> dinerIterator = dinerMenu.createIterator();

		System.out.println("MENU\n----\nBREAKFAST");
		printMenu(pancakeIterator);
		System.out.println("\nLUNCH");
		printMenu(dinerIterator);
	}
 
	private void printMenu(Iterator<MenuItem> iterator) {
		while (iterator.hasNext()) {
			MenuItem menuItem = iterator.next();
			System.out.print(menuItem.getName() + ", ");
			System.out.print(menuItem.getPrice() + " -- ");
			System.out.println(menuItem.getDescription());
		}
	}
 
	public void printVegetarianMenu() {
		System.out.println("\nVEGETARIAN MENU\n----\nBREAKFAST");
		printVegetarianMenu(pancakeHouseMenu.createIterator());
		System.out.println("\nLUNCH");
		printVegetarianMenu(dinerMenu.createIterator());
	}
 
	public boolean isItemVegetarian(String name) {
		Iterator<MenuItem> pancakeIterator = pancakeHouseMenu.createIterator();
		if (isVegetarian(name, pancakeIterator)) {
			return true;
		}
		Iterator<MenuItem> dinerIterator = dinerMenu.createIterator();
		if (isVegetarian(name, dinerIterator)) {
			return true;
		}
		return false;
	}


	private void printVegetarianMenu(Iterator<MenuItem> iterator) {
		while (iterator.hasNext()) {
			MenuItem menuItem = iterator.next();
			if (menuItem.isVegetarian()) {
				System.out.print(menuItem.getName());
				System.out.println("\t\t" + menuItem.getPrice());
				System.out.println("\t" + menuItem.getDescription());
			}
		}
	}

	private boolean isVegetarian(String name, Iterator<MenuItem> iterator) {
		while (iterator.hasNext()) {
			MenuItem menuItem = iterator.next();
			if (menuItem.getName().equals(name)) {
				if (menuItem.isVegetarian()) {
					return true;
				}
			}
		}
		return false;
	}
}
```

client
```java
public class MenuTestDrive {
	public static void main(String args[]) {
		PancakeHouseMenu pancakeHouseMenu = new PancakeHouseMenu();
		DinerMenu dinerMenu = new DinerMenu();
		Waitress waitress = new Waitress(pancakeHouseMenu, dinerMenu);
		//waitress.printMenu();
		// -- added 12/30/2016
		waitress.printMenu(1);
		// ---
		//waitress.printVegetarianMenu();

		System.out.println("\nCustomer asks, is the Hotdog vegetarian?");
		System.out.print("Waitress says: ");
		if (waitress.isItemVegetarian("Hotdog")) {
			System.out.println("Yes");
		} else {
			System.out.println("No");
		}
		System.out.println("\nCustomer asks, are the Waffles vegetarian?");
		System.out.print("Waitress says: ");
		if (waitress.isItemVegetarian("Waffles")) {
			System.out.println("Yes");
		} else {
			System.out.println("No");
		}

	}
}
```

## Composite Pattern 

[REF](https://fjp.at/design-patterns/composite)  

**The Composite Pattern allows us to build structures of objects in the form of trees that contain both compositions of objects and individual objects as nodes.**    
Using a composite structure, we can apply the same operations over both composites and individual objects. 
> In other words, in most cases we can ignore the differences between compositions of objects and individual objects.  

> Coomposite-Muster  
![](https://i.imgur.com/FddV3XT.png)  
![image](https://user-images.githubusercontent.com/68631186/126360058-040544ea-5d83-4431-8616-ad68f4eb3028.png)  


## Example
- A scenario for the menu inserting a new menu under it

We create a sub-menu(named dessert Menus) under dinermenu with tree-structure as the following  
![](https://i.imgur.com/Fc2nwQt.png)  

#### The goal we want is the following  
> 1. Print Whole Tree Structure   
> ![image](https://user-images.githubusercontent.com/68631186/126358373-a6853e2d-93a1-40e5-88c1-c9737ce8b575.png)  
> 2. Print Part of Tree Structure  
> ![image](https://user-images.githubusercontent.com/68631186/126358410-47defacc-3828-4783-b45c-8ab6f4ecbf81.png)  


#### The role of the MenuComponent abstract class is to provide an interface for the leaf nodes and the composite nodes.  
It should provide a default implementation of the methods so that if the MenuItem (the leaf) or the Menu(the composite) doesn't want to implement some of the methods.**(node and leaf extends same class but have different behaviour)**  
![](https://i.imgur.com/T4ENYIX.png)  
```java
public abstract class MenuComponent {
	public void add(MenuComponent menuComponent) {
		throw new UnsupportedOperationException();
	}
	public void remove(MenuComponent menuComponent) {
		throw new UnsupportedOperationException();
	}
	public MenuComponent getChild(int i) {
		throw new UnsupportedOperationException();
	}
  
	public String getName() {
		throw new UnsupportedOperationException();
	}
	public String getDescription() {
		throw new UnsupportedOperationException();
	}
	public double getPrice() {
		throw new UnsupportedOperationException();
	}
	public boolean isVegetarian() {
		throw new UnsupportedOperationException();
	}
  
	public void print() {
		throw new UnsupportedOperationException();
	}
}
```

Implementing the MenuItems(the Leaf)
```java
public class MenuItem extends MenuComponent{
	String name;
	String description;
	boolean vegetarian;
	double price;
    
	public MenuItem(String name, 
	                String description, 
	                boolean vegetarian, 
	                double price) 
	{ 
		this.name = name;
		this.description = description;
		this.vegetarian = vegetarian;
		this.price = price;
	}
  
	public String getName() {
		return name;
	}
  
	public String getDescription() {
		return description;
	}
  
	public double getPrice() {
		return price;
	}
  
	public boolean isVegetarian() {
		return vegetarian;
	}
  
	public void print() {
		System.out.print("  " + getName());
		if (isVegetarian()) {
			System.out.print("(v)");
		}
		System.out.println(", " + getPrice());
		System.out.println("     -- " + getDescription());
	}
}
```

Implementing the (composite)Menu
- Each menu has menu items stored in different types of collections (aggregates such as ArrayList, standard array or HashMap).
```java
import java.util.Iterator;
import java.util.ArrayList;
public class Menu extends MenuComponent {
	ArrayList<MenuComponent> menuComponents = new ArrayList<MenuComponent>();
	String name;
	String description;
  
	public Menu(String name, String description) {
		this.name = name;
		this.description = description;
	}
 
	public void add(MenuComponent menuComponent) {
		menuComponents.add(menuComponent);
	}
 
	public void remove(MenuComponent menuComponent) {
		menuComponents.remove(menuComponent);
	}
 
	public MenuComponent getChild(int i) {
		return (MenuComponent)menuComponents.get(i);
	}
 
	public String getName() {
		return name;
	}
 
	public String getDescription() {
		return description;
	}
 
	public void print() {
		System.out.print("\n" + getName());
		System.out.println(", " + getDescription());
		System.out.println("---------------------");
  
		Iterator<MenuComponent> iterator = menuComponents.iterator();
		while (iterator.hasNext()) {
			MenuComponent menuComponent = 
				(MenuComponent)iterator.next();
			menuComponent.print();
		}
	}
}
```

Now Waitress with composite pattern
```java
public class Waitress{
    MenuComponent allMenus;
    public Waitress(MenuComponent allMenus){
        this.allMenus = allMenu;
    }
    public void printMenu()
    {
        allMenus.print();
    }
}
```

Let's run 
```java
public class MenuTestDrive{
public static void main(String args[]) {
    MenuComponent pancakeHouseMenu = 
        new Menu("PANCAKE HOUSE MENU", "Breakfast");
    MenuComponent dinerMenu = 
        new Menu("DINER MENU", "Lunch");
    MenuComponent cafeMenu = 
        new Menu("CAFE MENU", "Dinner");
    MenuComponent dessertMenu = 
        new Menu("DESSERT MENU", "Dessert of course!");
    MenuComponent coffeeMenu = new Menu("COFFEE MENU", "Stuff to go with your afternoon coffee");

    MenuComponent allMenus = new Menu("ALL MENUS", "All menus combined");

    allMenus.add(pancakeHouseMenu);
    allMenus.add(dinerMenu);
    allMenus.add(cafeMenu);
    pancakeHouseMenu.add(new MenuItem(
            "K&B's Pancake Breakfast", 
            "Pancakes with scrambled eggs, and toast", 
            true,
            2.99));
            
    //... so on
    Waitress waitress = new Waitress(allMenus);
    waitress.printMenu();
            
}
```
- we use an internal iterator inside the Menu class to recursively to print the tree structure (of menus), because of that we actually have less control to iterate over components


## Discussion   
The Composite Pattern takes the Single Responsibility design principle and trades it for transparency.   

> One Single Responsibility
> 一個類別只能有一個改變的原因。你做你該做的事，我做我該做的事。你我互不干擾(e,g. menuItem and menu)  

> Transparency
> **By allowing the Component interface to contain the child management operations and the leaf operations**.  
> A client can treat both composites(Menu) and leaf nodes(MenusItem) uniformly; so whether an element is a composite node or leaf becomes transparent to the client(client have no further details about them).  

Given we have both types of operations in the Component class, we lose a bit of safety because a client might try to do something inappropriate or meaningless on an element (like try to add a menu to a menu item). 

This is a design decision; we could take the design in the other direction and separate out the responsibilities into interfaces.  
- This would make our design safe, in the sense that any inappropriate calls on elements would be caught at compile time or run-time, but we’d lose transparency and our code would have to use conditionals and the `instanceof` operator.  

## A Composite Iterator (External Iterator)  
To have more control for iterating like iterating all the items are vegetarian.  

we add new method `createIterator()` in the MenuComponent
```java
public interface MenuComponent{
    //...
    Iterator<MenuComponent> createIterator()
}

/**
  * <P> A menu contains many element (items) </p>
  */ 
public class Menu extends MenuComponent {
	Iterator<MenuComponent> iterator = null;
	ArrayList<MenuComponent> menuComponents = 
        new ArrayList<MenuComponent>();
	//...
  
	public Iterator<MenuComponent> createIterator() {
		if (iterator == null) {
			iterator = 
                new CompositeIterator(menuComponents.iterator());
		}
		return iterator;
	}
}

/**
  * <p> A menuItem is an element </p>
  * <p> An element can't be iterated </p>
  */
public class MenuItem extends MenuComponent{
    //....
    public Iterator<MenuComponent> createrIterator(){
        return new NullIterator()
    }
}
```

### The `Null` iterator

Because `MenuItem` has nothing to iterate over, so We got two choices to solve such problem  
1. return null from `createIterator()`
2. return an iterator that always return false whe `hasNext()` is called
```java
public class NullIterator implements <MenuComponent>{
    public Object next(){
        return null;
    }
    public boolean hasNext(){
        return false;
    }
    public void remove(){
        throw new UnsupportedOperationException();
    }
}
```

### To traverse the tree
Composite Iterator got the job of iterating over the MenuItems in the Component and of making sure all the child Menus(grandson Menus, and so on) are included
```java
public class CompositeIterator implements Iterator{
    Stack<Iterator<MenuComponent>> stack =
        new Stack<Iterator<MenuComponent>>();
    public CompositeIterator(Iterator iterator){
        stack.push(iterator);
    }
    //....
}
```

Now the waitress can use the composite iterator to show which items are vegetarian  
```java
public class Waitress{
    //...
    public void prinVegetarianMenu(){
        Iterator<MenuComponent> itr = allMenus.createIterator();
        //...
        while(itr.hasNext()){
            MenuComponent menuComponent = itr.next();
            try{
                if(menuComponent.isVegetarian()){
                    menuComponent.print();
                }
            }catch (UnspportedOperationException e) {}
        }
    }
}
```

To construct such tree structure   
![](https://i.imgur.com/PhWCSlw.png)    

