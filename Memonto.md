# Memento

###### tags: `Design Pattern`
[Ref : tutorialspoint](https://www.tutorialspoint.com/design_pattern/memento_pattern.htm)

> Pattern
> : ![](https://i.imgur.com/gGCZUOm.png)


Originator (Current State)
1. Creates a memento object capturing the originator's internal state.
2. Use the memento object to restore its previous states.

Caretaker 
1. Responsible for keeping the memento(s).
2. The memento is opaque to the caretaker, and the caretaker must not operate on it.

## CareTaker make a backup of multiple state  
![](https://i.imgur.com/f9V8vYW.png)





## Example Record/Get the sate of A character with HP and EXP using Memento pattern

[soruceCode](http://corrupt003-design-pattern.blogspot.com/2017/02/memento-pattern.html)

Originator
```java=
public class GamePlayer {
    // Current Sate
    // declare character's HP and exp
    private int mHp;
    private int mExp;

    public GamePlayer(int hp, int exp)
    {
        mHp = hp;
        mExp = exp;
    }

    // save the current state
    public GameMemento saveToMemento()
    {return new GameMemento(mHp, mExp);}

    // restore the previous state
    public void restoreFromMemento(GameMemento memento)
    {
        mHp = memento.getGameHp();
        mExp = memento.getGameExp();
    }

    public void play(int hp, int exp)
    {
        mHp = mHp - hp;
        mExp = mExp + exp;
    }
}
```
Memento
```java=
public class GameMemento {
    private int mGameHp;
    private int mGameExp;

    public GameMemento(int hp, int exp)
    {
        mGameHp = hp;
        mGameExp = exp;
    }

    public int getGameHp()
    {
        return mGameHp;
    }

    public int getGameExp()
    {
        return mGameExp;
    }
}
```

Caretaker ,
Consider it as an store that stores your state
```java=
// Caretaker (Care = Carry : saving/keeping the memnto)
public class GameCaretaker {

    private GameMemento mMemento;

    // get Memento
    public GameMemento getMemento()
    {
        return mMemento;
    }
    
    // save only one state
    public void setMemento(GameMemento memento)
    {
        mMemento = memento;
    }
}

public class Demo {

    public static void main(String[] args)
    {
        //create A new player
        GamePlayer player = new GamePlayer(100, 0);

        // SAVING the game
        GameCaretaker caretaker = new GameCaretaker();
        caretaker.setMemento(player.seveToMemento());

        // GG
        player.play(-100, 10);

        // reloading
        player.restoreFromMemento(caretaker.getMemento());
    }
}
```