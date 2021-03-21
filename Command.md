# Command

###### tags: `Design Pattern`

Simple Brief : The sender(invoker) sends the command( to the receiver a request to do the jobs


|Participants | dose something |
|-------------|--|
|Command      |declares an interface for executing an operation|
|ConcreteCommand| extends the Command interface, implementing the Execute method by invoking the corresponding operations on Receiver. It defines a link between the Receiver and the action
|Client       |creates a ConcreteCommand object and sets its receiver
|Invoker      |asks the command to carry out the request
|Receiver     | knows how to perform the operations


![](https://i.imgur.com/j3u2Wol.png)


```cpp=
/** Interface : command 
 **/
class command
{
    virtual void execute()=0;
}

/**Concrete Command
 **/
class command_1 : public command
{
private:
    receiver r;
public:
    // Make A Command to Receiver
    void execute(){
        r.action_1();
    }
}
class command_2 : public command
{
private:
    receiver r;
public:
    void execute(){
        r.action_2();
    }
}

/** To invoke the commands
    `Sending the command to receiver`
    `It controls the whole system`
 **/
class invoker 
{
private:
    command_1 _1;
    command_2 _2;
public:
    void Invokeaction_1(){
        _1.execute();
    }
    void Invokeaction_2(){
        _2.execute();
    }
}

/* the functions of devices 
 * e.g. Lamp TV etc...
 */
class receiver
{
public:
    // they can be called by Conrete Commands
    action_1(){
        // To do something
        // such as tunr on ... 
    }
    action_2(){
        //  ....
    }
    //.. other actions for the device ...
}
```
> How the commands working
> : Invokeaction() -> execute() -> action()


## combine multiple commands into one
We can drive the whole system on in class Invoker by using array

```cpp=
class Invoker
{
private:
    vector<unique_ptr<command>> ONcommands;
    vector<unique_ptr<command>> OFFcommands;
public:
    Invoker(int slots){
        // slots : total nums of devices
        ONcommands.resize(slots,nullptr);
        OFFcommands.resize(slot,nullptr);
    }
    void setCommand(int slot, 
              unique_ptr<command>>& ONcommands,
              unique_ptr<command>>& OFFcommands)
           {
               onCommands[slot] = Oncommands;
               offCommands[slot] = OFFcommands;
           }
    void onButtonWasPush(int slot){
        onCommands[slot].execute();
    }
    void offButtonWasPush(int slot){
        OFFCommands[slot].execute();
    }
    
    //.. other operations
    
}
```
