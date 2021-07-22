# State

###### tags: `Design Pattern`


> Pattern
> : ![](https://i.imgur.com/P5Ehwfs.png)

Context can request  each instance of different concrete states to do something(e.g ConcreteStateA.getsomething) and return these result to the client

## Example 
[SourceCode](https://github.com/bethrobson/Head-First-Design-Patterns/blob/master/src/headfirst/designpatterns/state/gumball/GumballMachine.java)  

A gumball machine without state pattern 
```java
public class GumballMachine {
	// define 4 states of Gumball Machine
	final static int SOLD_OUT = 0;
	final static int NO_QUARTER = 1;
	final static int HAS_QUARTER = 2;
	final static int SOLD = 3;

	int state = SOLD_OUT;
	int count = 0;

	public GumballMachine(int count) {
		this.count = count;
		if (count > 0) {
		    state = NO_QUARTER;
		}
	}

	/**
	* <p> Each Action would all content 4 states
	*     <li> HAS_QUARTER </li> 
	*     <li> NO_QUARTER  </li>
	*	    <li> SOLD_OUT    </li> 
	*	    <li> SOLD        </li>
	* </p>

	public void insertQuarter() 
	{
		if (state == HAS_QUARTER) {
		    //..
		} else if (state == NO_QUARTER) {
		    //..
		} else if (state == SOLD_OUT) {
		    //..
		} else if (state == SOLD) {
		    //..
		}
	}

	public void ejectQuarter() {
	//..
	}
	public void turnCrank() {
	//..
	}
	private void dispense() {
	//..
	}
```


If we are going to add a new function for gumball machine such as adding 10% of the time the sold state leads to two ball being release.
> By Adding a new state which means each stateAction (methods) in class `GumballMachine` should handle a new state, this means there will be a lot of code to modify 


A Gumball machine With State pattern 
```java
/**
  * <p> Abstraction stores 
  *     default states actions </p>
  */
public interface State {
 
	public void insertQuarter();
	public void ejectQuarter();
	public void turnCrank();
	public void dispense();
    
	public void refill();
}
/**
  *<p> Concrete State can be  
  *    the new state we will add in the future </p>
  *<p> Each state implements interface State 
  *    and override every defaults state </p>
  */
public class SoldoutState implements State{
    //...
}
public class SoldState implements State {
    //..
}
public class NoQuarterState implements State{
    //..
}
public class HasQuarterState implements State{
    //..
}

public class WinnerState implement State{
    //...
}

public class GumballMachine {
 
	State soldOutState;
	State noQuarterState;
	State hasQuarterState;
	State soldState;
	State winnerState;

	// default state
	State state = soldOutState;
	int count = 0;


	public GumballMachine(int numberGumballs) {
		soldOutState = new SoldOutState(this);
		noQuarterState = new NoQuarterState(this);
		hasQuarterState = new HasQuarterState(this);
		soldState = new SoldState(this);
		winnerState = new WinnerState(this);

		this.count = numberGumballs;
		if (numberGumballs > 0) {
			state = noQuarterState;
		} 
	}

	public void insertQuarter() {
		state.insertQuarter();
	}

	public void ejectQuarter() {
		state.ejectQuarter();
	}

	public void turnCrank() {
		state.turnCrank();
		state.dispense();
	}

	void setState(State state) {
		this.state = state;
	}

	void releaseBall() {
		System.out.println("A gumball comes rolling out the slot...");
		if (count > 0) {
			count = count - 1;
		}
	}
	// get gums
	int getCount() {
		return count;
	}

	// add the gums (count)
	void refill(int count) {
		this.count += count;
		System.out.println("The gumball machine was just refilled; its new count is: " + this.count);
		state.refill();
	}

	public State getState() {
		return state;
	}

	public State getSoldOutState() {
		return soldOutState;
	}

	public State getNoQuarterState() {
		return noQuarterState;
	}

	public State getHasQuarterState() {
		return hasQuarterState;
	}

	public State getSoldState() {
		return soldState;
	}

	public State getWinnerState() {
		return winnerState;
	}
}
```
Now Run our Gumball machine
```java
public class GumballMachineTestDrive
{
    public static void main(String[] args) {
        // A gumball Machine contents 10 gums 
        GumballMachine gumballMachine = new GumballMachine(10);
        
        // Now you
        // 1. insert a coin
        // 2. turn Crank
        // 3. insert a coin agian
        // 4. turn Crank
        gumballMachine.insertQuarter();
        gumballMachine.turnCrank();
        gumballMachine.insertQuarter();
        gumballMachine.turnCrank();
}
```

## Using of State and Strategy 

### Strategy 
In general, think of the Strategy Pattern as a flexible alternative to sub-classes;   
If you use inheritance to define the behavior of a class, then you’re stuck with that behavior even if you need to change it.  
With Strategy you can change the behavior by composing with a different object.  

### State 
Think of the State Pattern as an alternative to putting lots of conditionals in your context; by encapsulating the behaviors within state objects, you can simply change the state object in context to change its behavior.  
