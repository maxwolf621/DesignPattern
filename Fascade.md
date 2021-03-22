# Fassade-Muster

Mit dem Fascade-Muster können wir** ein komplexes Bassisytem nehmen und seine Verwendung vereinfach**

![](https://i.imgur.com/mvSPFnt.png)


## z.B Einen Film anschauen

Um einen Film zu schauen, müssen Sie ein paar einfach Aufgaben erliegen (komplexe Angelegenheit)
![](https://i.imgur.com/FXj8hPv.png)

Es wäre ein Chaos ohne dem hilf des Fassade-musters


## Fassade und Adapter

Sie können mehrere Klassen einpacken, aber der Zweck einer Fassade ist es, die Schnittstelle zu **vereinfachen**, der Zweck eines Adapters hingegen, die Schnittstelle in eine andere **umzuwandeln**


> Fassade 
> : vereinfachen
>
>Adapter 
>: umzuwandeln


## Das Prinzip der Verschwiegenheit

![](https://i.imgur.com/TUoAoZ5.png)

Dieses Prinzip hält uns davon ab, Entwürfe zu erstellen, in denen eine große Anzahl von Klassen so aneinander gekoppelt sind, dass sich Änderungen in einem Teil des System auf andere Teile auswirken.
**Wenn du viele Abhängigkeiten zwischen deinen Klassen aufbauen, baust du ein zerbrechenliches System auf, dessen Wartung teuer ist und das für andere sehr schwer zu verstehen ist**


So wie man sich keine Freunde macht? 
das Prinzip sagt uns dann,dass wir von jeder Methode in diesem Objekt nur Methoden aufrufen sollen, die
- zum Objekt selbst
- zu Objekten, die der Methode als Parameter übergebben
- zu Objekten, die die Methode erstellt oder 
:::info
Keine Methoden auf Objekten aufzurufen, die von Aufrufen anderer Methoden zurückgeliefert wurden
:::
- zu Komponenten des Objekts  (HAT-EINE-Beziehung)


Beispiel ohne das Prinzip
```java=
public float getTemperatur()
{
    Thermometer thermometer = station.getThermomter();
    /*ruft selbst dit Methode */
    retrun thermometer.getTemperatur();
}
```
Beispiel mit dem Prinzip

```java=
public float getTempratur()
{
    return station.getTemperatur();
}
```
> wir fügen der Klasse Station eine Methode hinzu, die für uns die Angrage an das Termometer richtet 
> Das Verringert die Anzahl der Klassen, von denen wir abhängig sind

```java=
public class Auto{
    Motor motor;
    // andere Instanzvariablen
    public Auto()
    {
        // objekt initialisieren
    }
    
    public void start(schlüssel schlüssel){
        // Erlaubt 
        // erstell ein neues Objekt
        Türen türen = new Türen();
        
        //Erlaubt
        //$die Methoden auf einem Objekt aufrufen
        //§ drehen() als Parameter übergeben wurde
        boolean autorisiert = schlüssel.drehen();
        if(autorisiert){
            //Erlaubt
            //Eine Methode auf einer Komponente des Objekts aufrufen
            motor.starten();
            //Erlaubt
            //Eine Lokale Methode des Objekts aufrufen
            cockpitAnzeigeAkutualisieren();
            
            //Erlaubt
            //eine Methode auf eiem Objekt anfrufen,
            //   das du erstellt oder instaiiert hast
            türen.schließen();
        }
    }
    pubic void cockpitAnzeigeAkutualisieren(){
        //....
    }
} 
```

## Das Fassade-Muster und das Prinzip der Verschwiegenheit

HomeTheaterFacade
> verwaltet alle komponenten des Basissystems für den Client.
>
> sie hält den Client einfach und flexibel

![](https://i.imgur.com/F6c6CVx.png)
