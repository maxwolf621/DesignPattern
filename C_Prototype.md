###### tags : `Design Pattern`

###### KeyWord : `clone` , `existing object` 
# Prototype 

[Example from cyc](https://github.com/CyC2018/CS-Notes/blob/master/notes/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%20-%20%E5%8E%9F%E5%9E%8B%E6%A8%A1%E5%BC%8F.md)   
[Example from iluwater](https://github.com/iluwatar/java-design-patterns/tree/master/prototype)  

Prototype is a creational design pattern that lets you **copy existing objects** without making your code **dependent** on their classes.  
- Create an object based on an existing object through cloning.  

> Pattern UML  
> ![image](https://user-images.githubusercontent.com/68631186/126876489-6ed3ce95-b8b5-4e13-bd5f-45a6609b70cc.png)   
> ![image](https://user-images.githubusercontent.com/68631186/126876603-d9e916e2-e589-4c67-9ce3-d1927cee5422.png)  

- The Prototype interface declares the cloning methods. In most cases, it’s a single clone method. 
- The Concrete Prototype class implements the cloning method. 
  > In addition to copying the original object’s data to the clone, this method may also handle some edge cases of the cloning process related to cloning linked objects, untangling recursive dependencies, etc.
- The Client can produce a copy of any object that follows the prototype interface.
