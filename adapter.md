# Adapter-Muster

###### tags: `Design Pattern`

> Konzept
> : ![](https://i.imgur.com/U8q6gUs.png)

Es gibt zwei Arten von Adaptern
1. Objekt-Adapter
2. Klassen-Adapter

Objekt- und Klassen-Adapter verwenden zwei verschiedene Mittel, um etwas zu adaptieren(Komposition bzw. Verbung).


## Objekt-Adapter

> Muster  
![](https://i.imgur.com/bMEvDzn.png)  
## Klassen-Adapter

**Java kann den nicht implementen**  

> Muster  
![](https://i.imgur.com/tf0toQx.png)


### Objekt- oder Klassen nutzet ?
(Das Beispiel steht auf die Seite 246)  

![](https://i.imgur.com/DkDzLFd.png)

![](https://i.imgur.com/N114x9r.png)
> Klasse DasAdapierte hat nicht die gliechen Methoden wie Ente aber der Adapter kann Methodenaufrufe für Ente annehmen unwandeln und an ihrer Stelle Methoden auf Truthahn aufrufen

![](https://i.imgur.com/zoZCVaQ.png)
> Dank des Adapters erhält Truthahn die Aufrufe, die der Client auf der Scnittstelle Ent macht


## Verwendung eines einfachen Adapters

Heute wir müssen uns mit alten Code herumschlagen,der die Enumerator-Schnittstelle anbietet, obwohl wir für unseren neuen Code eigentlich nur Iteratoren verwenden wollen

Alte Enumerations
![](https://i.imgur.com/2yLn0FK.png)

Neu Iteratoren
![](https://i.imgur.com/hCoeErm.png)





>![](https://i.imgur.com/FBaKQR7.png)
> Wir müssen herauszufiden, welche Methode wir auf der adaptierten Klasse aufrufen müssen, wenn der Client eine  Methode auf dem Ziel aufrugt


> UML
> : ![](https://i.imgur.com/Lu5XEbi.png)


Wir muss Enumeration an Iterator anpassen in unserem Adapter, so implementiert unser Adapter das Interface Iterator
```java=
public class EnumerationIterator implements Iterator<Object> {
    //Wir verwenden Komposition<?>, 
    //    speichern unser Referenz 
    //        also in einer Instanzvariablen 
    Enumeration<?> enumeration;
    public EnumerationIterator(Enumeration<?> enumeration){
        this.enumeration = enumeration;
    }

    // hastNext() von Iterator wird
    //    an die hasMoreElements() 
    //        von Enumeration delegiert
    public boolean hasNext() {
        return enumeration.hasMoreElements();
    }
    
    // next() von Iterator adaptiere nextElement() von Enumeration
    public Object next() {
        return enumeration.nextElement();
    }

    public void remove() {
        throw new UnsupportedOperationException();
    }
}
```
> Wir können die remove() von Iterator nicht unterstützen und müssen deswegen meckern, indem wir eine Exception auslösen



## Review von Musters

![](https://i.imgur.com/FI9LvjG.png)

