Simple brief : the sender send the command to the receiver a request to do the jobs


|Participants |  |
|-------------|--|
|Command      |declares an interface for executing an operation|
|ConcreteCommand| extends the Command interface, implementing the Execute method by invoking the corresponding operations on Receiver. It defines a link between the Receiver and the action
|Client       |creates a ConcreteCommand object and sets its receiver
|Invoker      |asks the command to carry out the request
|Receiver     | knows how to perform the operations



```cpp
#include <iostream>
#include <memory>

using namespace std;

/*the Command */
class Command 
{
public:
	virtual void execute()=0;
};

/*the Receiver : light */
class Light 
{
public:
	Light() {  }

	void turnOn() 
	{
		cout << "The light is on" << endl;
	}

	void turnOff() 
	{
		cout << "The light is off" << endl;
	}
};


/* the Concrete Command 
for turning on the light*/
class FlipUpCommand: public Command 
{
public:
  //constructor
	FlipUpCommand(Light& light):theLight(light){}
  
  virtual void execute(){theLight.turnOn();}

private:
	Light& theLight;
};

/* the Concret command
for turning off the light*/

class FlipDownCommand: public Command
{
public:   
	FlipDownCommand(Light& light) :theLight(light){}
	virtual void execute() {theLight.turnOff();}
private:
	Light& theLight;
};

/* Invoke 
send the command(request) to receiver class
*/
class Switch 
{
public:
	Switch(Command& flipUpCmd, Command& flipDownCmd)
	:flipUpCommand(flipUpCmd),flipDownCommand(flipDownCmd){}

	void flipUp(){flipUpCommand.execute();  }
  void flipDown(){flipDownCommand.execute();  }

private:
	Command& flipUpCommand;
	Command& flipDownCommand;
};
 
/*The test class or client*/
int main() 
{
  // receiver on
	Light lamp;
  // set the command
	FlipUpCommand switchUp(lamp);
	FlipDownCommand switchDown(lamp);

	Switch s(switchUp, switchDown);
	// send the command
  s.flipUp();
	s.flipDown();
}
```

