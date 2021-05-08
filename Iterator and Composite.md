
###### tags: `Design Pattern`

# Composite and Iterator
[TOC]


> Pattern
> : ![](https://i.imgur.com/murQQP1.png)



> Pattern
> : ![](https://i.imgur.com/FddV3XT.png)


## Scenario of merge different (types) menus
If we want to merge two menus 
>For example
>![](https://i.imgur.com/WwWcYtK.png)

```java=
public class MenuItem{
    String name;
    String description;
    boolean vegetarian;
    double price;
    
    public MenuItem(String name, String description,
                    boolean vegetarian, double price)
                    {
                        /* initialize ... */
                    }
    //...
}
public class PancakeHouseMenu{
    ArrayList<MenuItem> menuItems;
    
    public PancakeHouse(){
        menuItems = new ArrayList<MenuItem>();
        addItem(/* name, description, price */);
        //....
    }
    public void addItem(String name, String description,
                boolean vegetarian , double price)
                {
                    MenuItem menuItem = new MenuItem(/*..*/);
                    menItems.add(menuItem);
                }
}
public class DinerMenu{
    static final int MAX = 6;
    // nums of item
    int items = 0;
    MenuItem[] menuItems;
    
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
    > One uses array-list and the other uses array.

#### Now A waitress handle these menus sorta like this
```java=
public class waitress{
    PancakeHouseMenu pancakeHouseMenu =
        new PancakeHOuseMenu();
    // PancakeHouseMenu's format is ArrayList
    ArrayList<MenuItem> breakfastItems = 
        pancakeHouseMenu.getMenuItems();
    
    // dinerMenu's format is Array
    DinerMenu dinerMenu = new DinerMenu();
    MenuItem[] LunchItems = dinerMenu.getMenuItems();
    
    for(int i = 0 ; i < breakfastItems.size() ; ++i)
    {
        // using method of arraylist
        MenuItem menuItem = breakfastItems.get(i);
        // operations for 
        //   printing out name, price, Description
    }
    for(int i = 0; i < lunchItems.lengtho ; ++i){
        // using array
        MenuItem menuItem = lunchItem[i]
        // ...
    }
}
```
- here comes to the problem.
    > The Waitress need to have two different loop to iterate over the two different kinds of menus.  
    > Also if we added a third menu, we'd have yet another loop to iterate.  
    > **This definitely gives us (the Waitress) hard to maintain and extend (Against Design Pattern Rule)**

To solve such problem we can use `iterator pattern`
> Iterator
> : provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.
>
> : ![](https://i.imgur.com/aCNoJBB.png)

<font size=6>[SourceCode](https://github.com/bethrobson/Head-First-Design-Patterns/tree/master/src/headfirst/designpatterns/iterator/dinermerger)</font>



```java=
public interface Iterator{
    boolean hasNext()
    MenuItem next()
}
public class ArrayListIterator implements Iterator{
    ArrayList<MenuItem> items;
    int position = 0;
    //..
    public MenuItem next(){
        //..
    }
    public boolean hasNext(){
        //...
    }
}
public class ArrayIterator implements Iterator{
    MenuItem[] items;
    int position = 0;
    //..
    public MenuItem next(){
        //..
    }
    public boolean hasNext(){
        //...
    }
}
// To create our menu
public interface Menu{
    Iterator createIterator();
}

public class PancakeHouseMenu implements Menu{
    //..
    public Iterator createIterator(){
        return new PancakeIterator(menuItems);
    }
}
public class DinerMenu implements Menu{
    //..
}
```
-  Class MenuItem : Methods to set up name,description, price
-  Interface Menu : using methods in MenuItem to set up name,description, price and then create an Iterator and return it to the waitress
- Interface Iterator : different type menu can use by same methods(e.g next(), hasNext() ... etc)

Now the waitress can use the same loop method to iterate our menus
```java=
public class Waitress{
    // we can use Menu instead of concreate Menus
    //     Always programming to an interface,
    //        not  an implementation 
    //            less Decouple
    PanckaeHouseMenu pancakeHouseMenu;
    DinerMenu dinerMenu;
    
    public Waitress(PancakeHouseMenu pancakeHouseMenu,
            DinerMenu dinerMenu){
        this.pancakeHouseMenu = pancakeHouseMenu;
        this.dinerMenu = dinerMenu;
    }
    public void printMenu(){
        Iterator pancakeIter = pancakeHouseMenu.createIterator();
        Iterator dinerIter = dinerMneu.createIterator();
        //...
    }
    //..
}
```

## Using java.util.Iterator to resize our Menus

the New interface Iterator  
![](https://i.imgur.com/xTvQYut.png)
```java=
public interface Iterator{
    boolean hasNext()
    MenuItem next()
    // the remove element method
    void remove()
}
// for DinerMenuIterator
public class DinerMenuIterator implements Iterator{
    MenuItem[] list ;
    int position = 0;
    //..
    public void{
        //..
        if(list[position-1] != null){
        //To remove the last element of fixed Array
        // we need to shift up the array
            for(int i = position-1;i < (list.length)-1); ++i){
                list[i] = list[i+1]
            } 
            list[list.length-1] = null
        }
    }
}
```
Update our Waitress
```java=
public class Waitress{
    Menu pancakeHouseMenu;
    Menu dinerMenu;
    
    // Watiress holds the menus
    public Waitress(Menu pancakeHouseMenu,Menu dinerMenu){
        this.pancakeHouseMenu = pancakeHouseMenu;
        this.dinerMenu = dinerMenu;
    }
    public void printMenu(){
        //with java.util.Iterator 
        // and it also violates the open closed principle
        Iterator<MenuItem> pancakeIter = pancakeHouseMenu.createIterator();
        Iterator<MenuItem> dinerIter = dinerMneu.createIterator();
        
    }
    //..
}
```
To follow the open close principle 
```java=
public class Waitress{
    ArrayList<Menu> menus;
    
    public Waitress(ArrayList<Menu> menus){
        this.menus = menus;
    }
    public void printMenu(){
        Iterator<Menu> menuIterator = menu.iterator();
        while(menuIterator.hasNext())
        {
            Menu menu = menuIterator.next();
            printMenu(menu.createIteraotr());
        }
        void printMenu(Iterator<Menu> iterator){
            while(iterator.hasNext())
            {
                //....
            }
        }
        
    }
    //..
}
```
## A scenario for the menu inserting a new menu under it
[REF](https://fjp.at/design-patterns/composite)

If the Diner menu will create a sub-menu(dessert Menus) under it
![](https://i.imgur.com/Fc2nwQt.png)

To construct such tree structure we need the composite pattern
![](https://i.imgur.com/PhWCSlw.png)

First lets create the MenuComponent abstract class.
The role of the menu component is to provide an interface for the leaf nodes and the composite nodes.
MenuCompnent should provide a default implementation of the methods so that if the MenuItem (the leaf) or the Menu(the composite) doesn't want to implement some of the methods.

![](https://i.imgur.com/T4ENYIX.png)

```java=
public abstract class MenuComponent{
    /* methods add, remove, getChild will be grouped together
            as composite methods
    */
    public void add(MenuComponent menuConponent){
        throw new UnsupportedOperationException();
    }
    public void remove(/*...*/){
        // throw new Unsupported ...
    }
    public MenuComponent getChild(int i){
        // throw new Unsupported ....
    }
    
    // operations methods ...
    //    for such String getName(), string getDescription()
}
```

Implementing the MenuItems(the leaf)
```java=
public class MenuItem extends MenuComponent{
    String name ;
    String description;
    boolean vegetarian;
    double price;
    // Initialize our attributes
    public MenuItem( /*...*/){
        //...
    }
    
    // using the operation methods from base (MenuComponent)
    public Stirng getName(){
    //..
    }
    public String getDescription(){
    //..
    }
}
```
Implementing the Composite Menu
Each menu has menu items stored in different types of collections (aggregates such as ArrayList, standard array or HashMap).
```java=
import java.util.Iterator;
import java.util.ArrayList;
public class Menu extends MenuComponent {
	ArrayList<MenuComponent> menuComponents = 
        new ArrayList<MenuComponent>();
	String name;
	String description;
  
	public Menu(String name, String description) {
		this.name = name;
		this.description = description;
	}
    // the composite methods from MenuComponent
    // To add Menu items or other menu as sub menu
    //    to a Menu
	public void add(MenuComponent menuComponent) {
		menuComponents.add(menuComponent);
	}
 
	public void remove(MenuComponent menuComponent) {
		menuComponents.remove(menuComponent);
	}
 
	public MenuComponent getChild(int i) {
		return (MenuComponent)menuComponents.get(i);
	}
 
	//...
 
	public void print() {
		System.out.print("\n" + getName());
		System.out.println(", " + getDescription());
	    
        // we using iterator to traverse the tree 
		Iterator<MenuComponent> iterator = 
            menuComponents.iterator();
		while (iterator.hasNext()) {
			MenuComponent menuComponent = 
				(MenuComponent)iterator.next();
			menuComponent.print();
		}
	}
}
```

Now Waitress with composite pattern
```java=
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
```java=
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
> we use an internal iterator inside the Menu class to recursively to print the tree structure (of menus), because of that we actually have less control to iterate over components


## Discussion 

the Composite Pattern takes the Single Responsibility design principle and trades it for transparency. 

> Transparency
> : By allowing the Component interface to contain the child management operations and the leaf operations,
a client can treat both composites(Menu) and leaf nodes(MenusItem) uniformly; so whether an element is a composite or leaf node becomes transparent to the client(client have no further details about them).

Now given we have both types of operations in the Component class, we lose
a bit of safety because a client might try to do something inappropriate or
meaningless on an element (like try to add a menu to a menu item). 

This is a design decision; we could take the design in the other direction and separate
out the responsibilities into interfaces. This would make our design safe, in
the sense that any inappropriate calls on elements would be caught at compile
time or run-time, but we’d lose transparency and our code would have to use
conditionals and the instanceof operator.


## A Composite Iterator (External Iterator)

To have more control for iterating like iterating all the items are vegetarian

we add new method createIterator() in the MenuComponent
```java=
public interface MenuComponent{
    //...
    Iterator<MenuComponent> createIterator()
}

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
public class MenuItem extends MenuComponent{
    //....
    public Iterator<MenuComponent> createrIterator(){
        return new NullIterator()
    }
}
```

### The Null iterator

Because MenuItem has nothing to iterate over
so We got two choices to solve such problem
1. return null from createIterator()
2. return an iterator that always return false whe `hasNext()` is called
```java=
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


the Composite Iterator is a Serious iterator.
It's got the job of iterating over the MenuItems in the Component and of making sure all the child Menus(grandson Menus, and so on) are included
```java=
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

```java=
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

