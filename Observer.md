# Observer 

###### tags: `Design Pattern`

> The Observer Pattern 
> : defines a one-to-many dependency between objects so that when one object changes state, all of its dependents are notified and updated automatically. 
> ![](https://i.imgur.com/EVVQirN.png)
>
>Pattern
> : ![](https://i.imgur.com/CzsW8oV.png)


```python=
class subject:
    def __init__(self):
        self.observers = []
        self._state = None
        self._stateNum = 0
        # other states ...
    def registerObserver(self, observer):
        self.observers.append(observer)
    
    @property
    def GetState(self):
    # get state
        return self._state
    def GetStaeNum(self):
        return self._stateNum
    
    @product.setter
    def setState(self, value, valueNum):
    # set state
        self._state = value
        self._stateNum= value
        self._notifyObservers()
    def _notifyObservers(self):
    # update 
        for observer in self.observers:
            observer()
```


## Loose Coupling
The Observer Pattern provide an object design where subjects and observers are loosely coupled.

> When two objects are loosely coupled, they can interact,but have very little knowledge of each other


*Properties*
- The only thing the subject knows about an observer is that it implements a certain interface (the Observer interface). **Mean that The subject doesn’t need to know the concrete class of the observe**     
    > **So we can add/delete new observers at any time.**
    > -  Because ==the only thing the subject depends on is a list of objects that implement the Observer interface==, and we also can replace any observer at **run-time** with another observer and the subject will keep purring along.

- We never need to modify the subject to add new types of observers.

- **We can reuse subjects or observers independently of each other**
  > If we have another use for a subject or an observer, we can easily reuse them because the two aren’t tightly coupled.

- Changes to either the subject or an observer will not affect the other. Because the two are loosely coupled.

:::info
Design Principle
- Strive for loosely coupled designs between objects that interact.
:::

## Brief

Observer Pattern contains two parts

1. Interface Observable Subject
    > registerObserver(observer o) , add the Observer to list  
    > removeObserver(observer o), remove the Observer from list   
    > notifyObservers(type field), update each observer in the list  
2. Interface Observer 
    > update()
    > : this will be called by Observable Subject's method notifyObservers(type field) to update the fields


## Example of Design the Weather Station

![](https://i.imgur.com/PbT1KsS.png)


Three Interface Classes
1. Subject 
2. Observer
3. DisplayElement

#### interface Subject
```java
public interface Subject {
    // To add the observer
    public void registerObserver(Observer o);
    // to remove the obeserver
    public void removeObserver(Observer o);
    // Notify(update) obeservs 
    //    when the States of subject has Changed
    public void notifyObservers();
}
```
#### interface Observer
```java
public interface Observer {
	public void update(float temp, float humidity, float pressure);
}
```
#### interface DisplayElement
```java
public interface DisplayElement {
    public void display();
}
```

Implementing Subject interface 

[Good At using List instead of using ArrayList](https://stackoverflow.com/questions/2279030/type-list-vs-type-arraylist-in-java)

#### Subject



```java=
public class WeatherData implements Subject {
    // our observers List
    private List<Observer> observers;
    // observers need to contains these fields
    private float temperature;
    private float humidity;
    private float pressure;

    // subject constuctor will create list of observers
    public WeatherData() {
    // list of observers
        observers = new ArrayList<Observer>();
    }

    // these two methods will be called in observers's constructor
    public void registerObserver(Observer o) {
        observers.add(o);
    }
    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    /* 
    this will invoke when 
        setMeasurements(..)
           '-->measurementsChanged(..)
                '-->notifyObservers(..)
    */
    public void notifyObservers() {
        for (Observer observer : observers) {
            //when update is called
            //  display() would also be called
            observer.update(temperature, humidity, pressure);
        }
    }

    public void measurementsChanged() {
        notifyObservers();
    }

    public void setMeasurements(float temperature, 
                                float humidity, 
                                float pressure) 
    {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        measurementsChanged();
    }
    //... other methodes
}
```
 
#### Observers
```java=
public class CurrentConditionsDisplay implements Observer, DisplayElement 
{
    // notification from temperature
    private float temperature;
    private float humidity;
    private float pressure;
    
    
    // The Subject
    private WeatherData weatherData;
    public CurrentConditionsDisplay(WeatherData weatherData) {
        this.weatherData = weatherData;
        // add this observer to array list observers
        weatherData.registerObserver(this);
    }

    // this will be called by methode in class Subject
    public void update(float temperature, 
                       float humidity, 
                       float pressure)
    {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        display();
    }

    public void display() {
        System.out.println("Current conditions: " +
                            emperature + 
                            "F degrees and " + 
                            humidity + "% humidity");
    }
}
```

- we can also have another Display such as ForecastDisplay... etc 

Now run our Weather Station
```java=
public class WeatherStation {
    public static void main(String[] args) {
        // create the subject
        WeatherData weatherData = new WeatherData();

        // add observer into array
        CurrentConditionsDisplay currentDisplay =  new CurrentConditionsDisplay(weatherData);
        
        /* more observer s*/
        //StatisticsDisplay statisticsDisplay = new StatisticsDisplay(weatherData);
        //ForecastDisplay forecastDisplay = new ForecastDisplay(weatherData);

        // Subjects updates three times 
        //    observer will know 
        weatherData.setMeasurements(80, 65, 30.4f);
        weatherData.setMeasurements(82, 70, 29.2f);
        weatherData.setMeasurements(78, 90, 29.2f);

        // remove the observer
        weatherData.removeObserver(currentDisplay);

	}
}
```


## Java build-in Observer Pattern

![](https://i.imgur.com/Onrd9pB.png)
