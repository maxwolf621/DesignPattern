# Observer 

> The Observer Pattern 
> : defines a one-to-many dependency between objects so that when one object changes state, all of its dependents are notified and updated automatically.
>
>Pattern
> : ![](https://i.imgur.com/CzsW8oV.png)



Key Point
1. When the state of one object changes, all of its dependents are notified. ![](https://i.imgur.com/EVVQirN.png)
2. The Observer Pattern provides an object design where subjects and observers are loosely coupled.




## Loose Coupling

When two objects are loosely coupled, they can interact,but have very little knowledge of each other.

Reason 
1. The only thing the subject knows about an observer is that it implements a certain interface (the Observer interface). 
It doesn’t need to know the concrete class of the observe
2. We can add/delete new observers at any time. 
Because the only thing the subject depends on is a list of objects that implement the Observer interface, and we also can replace any observer at run-time with another observer and the subject will keep purring along.
3. We never need to modify the subject to add new types of observers. Let’s say we have a new concrete class come along that needs to be an observer. 
4. We can reuse subjects or observers independently of each other. If we have another use for a subject or an observer, we can easily reuse them because the two aren’t tightly coupled.
5. Changes to either the subject or an observer will not affect the other. Because the two are loosely coupled.

:::info
Design Principle
Strive for loosely coupled designs between objects that interact.
:::


## Example of Design the Weather Station

![](https://i.imgur.com/PbT1KsS.png)


Interface Classes
> - Subject
> - Observer
> - DisplayElement
```java
public interface Subject {
    // To remove or add observer
    public void registerObserver(Observer o);
    public void removeObserver(Observer o);
    // Notify obeservs 
    //    when the State of subject has Changed
    public void notifyObservers();
}

public interface Observer {
	public void update(float temp, float humidity, float pressure);
}

public interface DisplayElement {
    public void display();
}
```

Implementing Subject interface 
```java=
public class WeatherData implements Subject {
    private List<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherData() {
    // list of observers
        observers = new ArrayList<Observer>();
    }

    // It will be used in observers's constructor
    public void registerObserver(Observer o) {
        observers.add(o);
    }
    public void removeObserver(Observer o) {
        observers.remove(o);
    }


    // invoke when 
    //  setMeasurements(..)
    //   '-->measurementsChanged(..)
    //        '-->notifyObservers(..)
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

    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        measurementsChanged();
    }
    //... other methodes
}
```
 
Observers
```java=
public class CurrentConditionsDisplay 
    /* impelements two interface */
    implements Observer, DisplayElement 
{
    private float temperature;
    private float humidity;
    private float pressure;
    private WeatherData weatherData;

    public CurrentConditionsDisplay(WeatherData weatherData) {
        this.weatherData = weatherData;
        // add observer to array list observers
        weatherData.registerObserver(this);
    }

    // it will called by methode in Subject
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
// we can also have another Display such as ForecastDisplay... etc ...
```

Now run our Weather Station
```java=
public class WeatherStation {
    public static void main(String[] args) {
        // create our subject
        WeatherData weatherData = new WeatherData();

        // add currentDisplay into the observers-list in weatherData
        CurrentConditionsDisplay currentDisplay =  new CurrentConditionsDisplay(weatherData);
        
        /* more observer s*/
        //StatisticsDisplay statisticsDisplay = new StatisticsDisplay(weatherData);
        //ForecastDisplay forecastDisplay = new ForecastDisplay(weatherData);

        // update three times 
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
