```c
class Doctor
{
private:
    vector<Patient *> m_patient {};
public:
    Doctor();
   //it's publicly exposed
   void addPatient(Patient *pat);
};
 
class Patient

private:
    vector<Doctor *> m_doctor{}; // so that we can use it here
    
    // make addDoctor private because we don't want the public to use it.
    // They should use Doctor::addPatient() instead, which is publicly exposed
    void addDoctor(Doctor *doc)
    {
        m_doctor.push_back(doc);
    }
 
public:
    Patient();
    // We'll friend Doctor::addPatient() 
    // so it can access the private function Patient::addDoctor()
    friend void Doctor::addPatient(Patient *pat);
};
```


## Inheritance
```cpp
//...
```
## Realization 
```
| class with pure virtual functions |->| class z that implements pure virtual functions |
                              '------->| class x that implements pure virtual functions |
                              '------->| class y that implements pure virtual functions |
```

```cpp
class Shape   
// An interface class only contains pure virtual functions
{
  public:
    virtual ~shape();
    virtual void move_x(int x) = 0;
    virtual void move_y(int y) = 0;
    virtual void draw() = 0;
};
class Line : public Shape
{
  public:
    virtual ~line();
    virtual void move_x(int x); // implements move_x
    virtual void move_y(int y); // implements move_y
    virtual void draw();        // implements draw
  private:
    point end_point_1, end_point_2;
};
```



## Dependency
Dependency: 
Dependencies typically are not represented at the class level — that is, 
The object being depended on is not linked as a member. Rather, the object being depended on is typically instantiated as needed. 

e.g. Our classes that use std::cout to accomplish the task of printing something to the console.
     | class object | -> | other class to do something |

| Point obj that uses | 
  '----> | `std::ostream& operator<<()`|

```cpp
class Point
{
private:
    double x,y;
pbulic:
  Point();
  friend std::ostream& operator<< (std::ostream &out, const Point &point);
};

std::ostream& operator<<(std::ostream &out, const Point &point)
{
  out<<"Point("< point.m_x<< ", "<<point.m_y; 
  return out;
}
```



