# Factory 

Product -> concrete product -> factory -> method to call factory -> client
A factory can create concreete product's obj (using enum) that implemntes from Product while it's asked by a client


So first
To create the product and then concrete product

|Prodcut   | concrete product  | 
|----------| ------------------| 
|Pizza     |HamAndMushRoomPizza|
|          | DeluxePizza       |
|          | HawaiianPizza     | 

```cpp
#include <iostream>
#include <memory>
using namespace std;

class Pizza {
public:
	virtual int getPrice() const = 0;
	virtual ~Pizza() {};  /* without this, no destructor for derived Pizza's will be called. */
};

class HamAndMushroomPizza : public Pizza {
public:
	virtual int getPrice() const { return 850; };
	virtual ~HamAndMushroomPizza() {};
};

class DeluxePizza : public Pizza {
public:
	virtual int getPrice() const { return 1050; };
	virtual ~DeluxePizza() {};
};

class HawaiianPizza : public Pizza {
public:
	virtual int getPrice() const { return 1150; };
	virtual ~HawaiianPizza() {};
};
```

A method to call(create) different type of pizza (using enum)
```cpp 
class PizzaFactory {
public:
	enum PizzaType {
		HamMushroom,
		Deluxe,
		Hawaiian
	};
	static unique_ptr<Pizza> createPizza(PizzaType pizzaType) {
		switch (pizzaType) {
		case HamMushroom: return make_unique<HamAndMushroomPizza>();
		case Deluxe:      return make_unique<DeluxePizza>();
		case Hawaiian:    return make_unique<HawaiianPizza>();
		}
		throw "invalid pizza type.";
	}
};
```

Now the client can ask for something ...
```cpp
int main()
{
  /*Create all available pizzas and print their prices*/
  void pizza_information(PizzaFactory::PizzaType pizzatype)
  {
    unique_ptr<Pizza> pizza = PizzaFactory::createPizza(pizzatype);
    cout << "Price of " << pizzatype << " is " << pizza->getPrice() << std::endl;
  }
	pizza_information(PizzaFactory::HamMushroom);
	pizza_information(PizzaFactory::Deluxe);
	pizza_information(PizzaFactory::Hawaiian);
}
```
