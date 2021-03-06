###### tags: `Design Pattern`
# Command
[Java version](https://github.com/CyC2018/CS-Notes/blob/master/notes/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%20-%20%E5%91%BD%E4%BB%A4.md)

The `invoker` execute the `command` to the `receiver` a certain request to operate a certain task

For example
- Pass a button on a remote(`invoker`) to turn off(`command`) a lamp(`receiver`)

|Participants    | dose something |
|-------------   |  --------------|
|Command         |declares an interface for executing an operation|
|Concrete Command|implements the Command interface, implementing the Execute method by invoking the corresponding operations on Receiver. **It defines a link between the Receiver and the action**
|Client          | Creates a Concrete Command object and sets its receiver
|Invoker         | Asks the command to carry out the request
|Receiver        | Performs the operations for tasks

![](https://i.imgur.com/j3u2Wol.png)


```cpp
/** 
 * command Interface 
 **/
class command
{
    virtual void execute()=0;
}

/**
 * Concrete Command
 */
class command_1 : public command
{
private:
    receiver r;
public:
    
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



/** 
 * We can image invoker as a remote control 
 * It invokes the command to operate a certain task
 */
class invoker 
{
private:
    command_1 _1;
    command_2 _2;
public:
    /**
     * Invokeaction_x
     *   Invoke an Action
     *   For Sepecific Command 
     *   for specific Receiver (Production)
     */
     
    void Invokeaction_1(){
        _1.execute();
    }
    void Invokeaction_2(){
        _2.execute();
    }
}

/**
 * A function fot a production
 * e.g lamp ... etc
 */
class receiver
{
public:
    /**
     * <p> actions are called by
     *     concret command </p>
     * <p> Image the following methods as 
     *     Function of the Productions </p>
     */ 
     
    action_1(){
        // To do something
        // such as turn on the lamp ... 
    }
    action_2(){
        //  turn off the light ...
    }
    //.. other actions for the device ...
}
```



## `Invoker` combines multiple commands into one

```cpp
class Invoker
{
private:
    vector<unique_ptr<command>> ONcommands;
    vector<unique_ptr<command>> OFFcommands;
public:
    Invoker(int slots){
        // slots : total num of the buttons
        ONcommands.resize(slots,nullptr);
        OFFcommands.resize(slots,nullptr);
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

