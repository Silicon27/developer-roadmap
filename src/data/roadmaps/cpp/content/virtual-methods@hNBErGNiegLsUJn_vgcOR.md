> Virtual functions are member functions whose behavior can be overridden in derived classes.

Take the example:
```cpp
#include <iostream>

class Base {
public:
	virtual void virFunc()
    {
        std::cout << "Base class\n";
    }
};

class Derived : public Base {
public:
	void virFunc() override // override here is optional
	{
		std::cout << "Derived class\n";
	}
};
```

Now we can attempt to use them:
```cpp
int main(){
	Base b;
	Derived d;

	// virtual member function call through reference
	Base& baseReference = b;
	Base& derivedReference = d;

	baseReference.virFunc(); // gives "Base class"
	derivedReference.virFunc(); // gives "Derived class"
	

	// virtual member function call through pointer
	Base* basePointer = &b;
    Base* derivedPointer = &d;
    
    basePointer->virFunc(); // prints "Base class"
    derivedPointer->virFunc(); // prints "Derived class"

	
	return 0;
}
```

Now the difference between a non-virtual and a `virtual` function is that virtual functions allow for the correct member function to be called when the type of the object is not exactly the derived class, but instead a pointer to the base class it was derived from.

Therefore, if `d` is a `Derived` object, then `Base* dp = &d` is a pointer to the base class even though the object itself is referring to the `Derived` class.

To put it more simply, if:
```cpp
class Base;
class Derived : Base;
```

and, 
```cpp
class Base {
public: 
	void print() { // non-virtual
		std::cout << "Base\n";
	}
};

class Derived : public Base {
public: 
	// This defines a new `print` function that hides Base::print().
	// It will only be called when using a Derived object directly,
	// not through a Base pointer or reference (unless Base::print is virtual).
	void print() { 
		std::cout << "Derived\n";
	}
};
```

then:
```cpp
int main() {
	Derived d;

	Base* bp = &d;
	
	// Because print() is not virtual, this call resolves at compile time to Base::print().
	bp->print(); 
}
```

Calling a function through a base class pointer or reference that refers to a derived object is known as **static dispatch** when the function is not virtual. This is determined at compile time.
```cpp
int main() {
	Derived d;
	
	Base* bp = &d;
	bp->print(); // static dispatch (if Base::print() is non-virtual)
}
```

If the function were marked `virtual`, this would instead be **dynamic dispatch** — the actual function called would be determined at runtime based on the object’s dynamic type.
