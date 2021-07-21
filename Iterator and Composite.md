
###### tags: `Design Pattern`
# Iterator and Composite pattern

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
/**
  * <p> Iterator from java package </p>
  */
public interface Iterator{
    boolean hasNext()
    MenuItem next()
    void remove() // the remove element method 
}

/**
  * we add {@code remove()} method in our DinerMenuIterator
  */
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


/**
  * <p> update the Waitress </p>
  */
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

/**
  * <p> client </p>
  */
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
[Ref_inTW](https://dotblogs.com.tw/pin0513/2011/01/16/20838)

By applying iterator pattern we break open closed principle in `Waitress` class
```java
public class Waitress{
    Menu pancakeHouseMenu;
    Menu dinerMenu;
    Menu dessert;
    
    //..
    
    public void printMenu(){
        /**
          * <p> The following declaration violates the open closed principle </p>
	  * <p> Once we get a new menu we must add 
	  *     a new {@code Iterator<MenuItem>} again. <p>
	  */
        Iterator<MenuItem> pancakeIter = pancakeHouseMenu.createIterator();
        Iterator<MenuItem> dinerIter = dinerMneu.createIterator();
	
	Iterator<MenuItem> dessert = dessertMenu.createIterator(); // add new Menu
        
	// ...
    }
    //..
}
```
- For such problem we can use composite pattern

> Coomposite-Muster  
![](https://i.imgur.com/FddV3XT.png)  
![image](https://user-images.githubusercontent.com/68631186/126360058-040544ea-5d83-4431-8616-ad68f4eb3028.png)  
- **The Composite Pattern allows us to build structures of objects in the form of trees that contain both compositions of objects and individual objects as nodes.**  

## Example
A scenario for the menu inserting a new menu under it  

We create a sub-menu(named dessert Menus) under dinermenu with tree-structure as the following  
![](https://i.imgur.com/Fc2nwQt.png)  

#### The goal we want is the following  
> 1. Print Whole Tree Structure   
> ![image](https://user-images.githubusercontent.com/68631186/126358373-a6853e2d-93a1-40e5-88c1-c9737ce8b575.png)  
> 2. Print Part of Tree Structure  
> ![image](https://user-images.githubusercontent.com/68631186/126358410-47defacc-3828-4783-b45c-8ab6f4ecbf81.png)  
- Using a composite structure, we can apply the same operations over both composites and individual objects.  
	> In other words, in most cases we can ignore the differences between compositions of objects and individual objects.(e.g. print whole structure or just individual ones.)  

#### The role of the MenuComponent abstract class 
As we can see from pattern it provides an interface for the leaf nodes and the composite nodes.  
- **it should provide a default implementation of the methods** so that if the MenuItem (the leaf) or the Menu(the composite) doesn't want to implement some of the methods.**(node and leaf extends same class but have different behaviour)**  
![](https://i.imgur.com/T4ENYIX.png)  

For example
```java
/**
  * <p>  define our default implementation of the methods </p>
  */
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

/**
  * <p> The Leaf </p>
  * <p> It needs </p>
  * <li>{@code getName} </li>
  * <li>{@code getDescription} </li>
  * <li>{@code getPrice} </li>
  * <li>{@code isVegetarian} </li>
  * <li>{@code print} </li>
  */
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
  
        /**
	  * @Description
	  *    Print this the item on a certain menu
	  *   <li>name</li>
	  *   <li>price</li>
	  *   <li>description</li>
	  */
	public void print() {
		System.out.print("  " + getName());
		if (isVegetarian()) {
			System.out.print("(v)");
		}
		System.out.println(", " + getPrice());
		System.out.println("     -- " + getDescription());
	}
}


/**
  * <p> Composite Objects </p>
  * <p> Each menu(Component) has different items stored 
  * 	in different types of collections 
  * 	(aggregates such as ArrayList, standard array or HashMap). </p>
  * <p> It needs to implement the following methods </p>
  * <li> {@code add} </li>
  * <li> {@code remove} </li>
  * <li> {@code getChild} </li>
  * <li> {@code getName} </li>
  * <li> {@code getDescription} </li>
  * <li> {@code print} </li>
  */
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
 
	/**
	  * @Description
	  *   Print The Menu's
	  *   <li> name <li>
	  *   <li> description <li>
	  *   and iterate each items in this menu
	  */
	public void print() {
		System.out.print("\n" + getName());
		System.out.println(", " + getDescription());
		System.out.println("---------------------");
  	
		/**
		  * <p> Iterate of an Array List via {@code iterator()} </p>
		  */ 
		Iterator<MenuComponent> iterator = menuComponents.iterator();
		while (iterator.hasNext()) {
			MenuComponent menuComponent = (MenuComponent)iterator.next();
			menuComponent.print();
		}
	}
}
```

Now Waitress with composite pattern which sticks to the open closed principal
```java
public class Waitress{
    /**
      * <p> All menus contains 
      *     in the Tree Structure </p>
      */
    MenuComponent allMenus;
    
    public Waitress(MenuComponent allMenus){
        this.allMenus = allMenu;
    }
    public void printMenu()
    {
    	/**
	  * Compare with adding {@code Iterator<MenuIitem>}
	  * for each menu 
	  */
        allMenus.print();
    }
}

/**
  * <p> Client </p>
  */
public class MenuTestDrive{
public static void main(String args[]) {
    
    /**
      * Create Menus 
      * <li> PACAKE HOUSE MENU </li>
      * <li> DINER MENU </li>
      * <li> CAFE MENU </li>
      * <li> DESSERT MENU </lI>
      */
    MenuComponent pancakeHouseMenu = 
        new Menu("PANCAKE HOUSE MENU", "Breakfast");
    MenuComponent dinerMenu = 
        new Menu("DINER MENU", "Lunch");
    MenuComponent cafeMenu = 
        new Menu("CAFE MENU", "Dinner");
    MenuComponent dessertMenu = 
        new Menu("DESSERT MENU", "Dessert of course!");
    MenuComponent coffeeMenu = new Menu("COFFEE MENU", "Stuff to go with your afternoon coffee");

    /**
      * Put all the menus in allMenus
      */
    MenuComponent allMenus = new Menu("ALL MENUS", "All menus combined");
    allMenus.add(pancakeHouseMenu);
    allMenus.add(dinerMenu);
    allMenus.add(cafeMenu);
    
    /**
      * Create a menu under a menu
      */
    pancakeHouseMenu.add(new MenuItem(
            "K&B's Pancake Breakfast", 
            "Pancakes with scrambled eggs, and toast", 
            true,
            2.99));
            
    /**
      * Ask Waitress show the menus
      */
    Waitress waitress = new Waitress(allMenus);
    waitress.printMenu();
            
}
```
- we use an internal iterator inside the Menu class to recursively to print the tree structure (of menus), because of that we actually have less control to iterate over components


## Choose OSR or Transparency for Composite pattern?
[Detail](https://ithelp.ithome.com.tw/articles/10242929)

**The Composite Pattern takes the Single Responsibility design principle and trades it for transparency.**  



### One Single Responsibility
一個類別只能有一個改變的原因。你做你該做的事，我做我該做的事。你我互不干擾

#### Composite Pattern that uses OSR  
![image](https://user-images.githubusercontent.com/68631186/126422735-b5c4efc5-d836-4320-8971-003ae86c9143.png)  

For example  
Although MenuItem and MenuComponent both extend the same abstract class but  
- `MenuItem` is leaf ( it doesn't implement add/remove method)  
- `MenuComponent` is Composite Objects (it implements add/remove method)  
 
### Transparency
**By allowing the Component interface to contain the child management operations and the leaf operations**.  

#### Composite Pattern that uses Transparency
![image](https://user-images.githubusercontent.com/68631186/126422678-b8a4975f-3f28-4c05-b26b-d97f32b535c1.png)

**A client can treat both composites(MenuCoponent) and leaf nodes(MenusItem) uniformly**;  so whether an element is a composite node or leaf becomes transparent to the client(client have no further details about them).  

**Given we have both types of operations in the Component class, we lose a bit of safety because a client might try to do something inappropriate or meaningless on an element (e.g. leaf can call `remove` method).**  

### Descision 

It is a design decision; we could take the design in the other direction and separate out the responsibilities into interfaces.  
This would make our design safe, in the sense that any inappropriate calls on elements would be caught at compile time or run-time, but we’d lose transparency and our code would have to use conditionals and the `instanceof` operator.  

## The `Composite` with `Iterator` pattern (External Iterator)  
To have more control to iterate all the items which are vegetarian.  

we add new method `createIterator()` in the MenuComponent
```java
public interface MenuComponent{
    // ...
    Iterator<MenuComponent> createIterator()
}


public class Menu extends MenuComponent {
	Iterator<MenuComponent> iterator = null;
	ArrayList<MenuComponent> menuComponents = new ArrayList<MenuComponent>();
	
	//...
  
        
	public Iterator<MenuComponent> createIterator() {
		if (iterator == null) {
			/**
			  * <p> Push the the menuComponent to the stack </p>
			  */
			iterator = new CompositeIterator(menuComponents.iterator());
		}
		return iterator;
	}
}

/**
  * <p> A menuItem is an element(the leaf) </p>
  * <p> An element can't be iterated </p>
  */
public class MenuItem extends MenuComponent{
    
    //....
    
    /**
      * @Description
      *     In case a item object uses {@code createIterator}
      * @return Null iterator
      */
    public Iterator<MenuComponent> createrIterator(){
        return new NullIterator()
    }
}
```

### null iterator implementation

Because `MenuItem` has nothing to iterate over, so we got two choices to solve such problem  
1. Return `null` from `createIterator()`
2. Return an iterator that always return false whe `hasNext()` is called
```java
public class NullIterator implements Iterator<MenuComponent>{

	public Object next(){
		return null;
	}
	public boolean hasNext(){
		return false;
	}
	
    	/*
	 * No longer needed as of Java 8
	 * 
	 * (non-Javadoc)
	 * @see java.util.Iterator#remove()
	 * 
	public void remove() {
		throw new UnsupportedOperationException();
	}
	*/
}
```

### CompositeIterator

Composite Iterator got the job of iterating over the MenuItems in the Component and of making sure all the child Menus(grandson Menus, and so on) are included
```java
public class CompositeIterator implements Iterator{
	Stack<Iterator<MenuComponent>> stack = new Stack<Iterator<MenuComponent>>();
   
	public CompositeIterator(Iterator<MenuComponent> iterator) {
		stack.push(iterator);
	}
   
	public MenuComponent next() {
		if (hasNext()) {
			Iterator<MenuComponent> iterator = stack.peek();
			MenuComponent component = iterator.next();
			stack.push(component.createIterator());
			return component;
		} else {
			return null;
		}
	}
  
	public boolean hasNext() {
		if (stack.empty()) {
			return false;
		} else {
			Iterator<MenuComponent> iterator = stack.peek();
			if (!iterator.hasNext()) {
				stack.pop();
				return hasNext();
			} else {
				return true;
			}
		}
	}
}
```

Now the waitress can use the composite iterator to show which items are vegetarian  
```java
public class Waitress{
	
	//...
	
	public void printVegetarianMenu() {
		Iterator<MenuComponent> iterator = allMenus.createIterator();

		System.out.println("\nVEGETARIAN MENU\n----");
		while (iterator.hasNext()) {
			MenuComponent menuComponent = iterator.next();
			try {
				if (menuComponent.isVegetarian()) {
					menuComponent.print();
				}
			} catch (UnsupportedOperationException e) {}
		}
	}
}
```

client
```java
public class MenuTestDrive {
	public static void main(String args[]) {
		
		/**
		  * Create the Menu Components
		  */
		MenuComponent pancakeHouseMenu = 
			new Menu("PANCAKE HOUSE MENU", "Breakfast");
		MenuComponent dinerMenu = 
			new Menu("DINER MENU", "Lunch");
		MenuComponent cafeMenu = 
			new Menu("CAFE MENU", "Dinner");
		MenuComponent dessertMenu = 
			new Menu("DESSERT MENU", "Dessert of course!");
  	  	/**
		  * Merge the menu togetehr
		  */
		MenuComponent allMenus = new Menu("ALL MENUS", "All menus combined");
		allMenus.add(pancakeHouseMenu);
		allMenus.add(dinerMenu);
		allMenus.add(cafeMenu);
  
  		/**
		  * <strong> add the items in the menus </strong>
		  */
		pancakeHouseMenu.add(new MenuItem(
			"K&B's Pancake Breakfast", 
			"Pancakes with scrambled eggs and toast", 
			true,
			2.99));

		//...
		
		dinerMenu.add(new MenuItem(
			"Vegetarian BLT",
			"(Fakin') Bacon with lettuce & tomato on whole wheat", 
			true, 
			2.99));
	
		//....
   
   		/**
		  * <strong> Add the MenuComponent under a MenuComponent </strong>
		  */
		dinerMenu.add(dessertMenu);
  
		dessertMenu.add(new MenuItem(
			"Apple Pie",
			"Apple pie with a flakey crust, topped with vanilla icecream",
			true,
			1.59));
		//...
		
		cafeMenu.add(new MenuItem(
			"Veggie Burger and Air Fries",
			"Veggie burger on a whole wheat bun, lettuce, tomato, and fries",
			true, 
			3.99));
		//...
 
 		/**
 	          * Ask The Watiress for menu
		  */
		Waitress waitress = new Waitress(allMenus);  
		
		waitress.printVegetarianMenu();  // ask the waitress for Vegetarian Menu
		
	}
}
```


To construct such tree structure   
![](https://i.imgur.com/PhWCSlw.png)    

