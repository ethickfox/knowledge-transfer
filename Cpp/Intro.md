# Header files
These usually have definitions of objects and declarations of global functions.
A header file (.h) defines the interface to the class, which includes:
- ﻿﻿Declaration of all member variables
- ﻿﻿Declaration of all member functions

Needed for all header files. Instructs compiler to compile code only once
``` cpp
#pragma once
```
Header example
``` cpp
class Cube {
	public:
		double getVolume () :
		double getSurfaceArea () ;
		void setLength (double length) ;
		
	private:
	double length_;
};
```

# Implementation file
An implementation file (.cpp) contains the code to implement the class (or other C++ code).
``` cpp
#include "Cube.h"

double Cube::getVolume() {
	return length_ * length_ * length_;
}

double Cube::getSurfaceArea() {
	return 6 * length_ * length_;
}
void Cube::setLength(double length) {
	length_ = length;
}
```

main.cpp
``` cpp
#include "Cube.h"

int main() {
	Cube c;
	c.setLength(3.48);
	
	double volume = c.getVolume();
	std::cout << "Volume:" << volume << std::endl;
	return 0;
}

```

This time there are two `#include` directives! First, it includes a standard library header from the system directory. This is shown by the use of **< >**. When you write `#include <iostream>`, the compiler will look for the **iostream** header file in a system-wide library path that is located outside of your current directory.
``` cpp
#include <iostream>
#include "Cube.h"
```
The compiled object files are allowed to rely on _definitions_ that appear in the other object files. That's why it's okay that main.cpp doesn't have the Cube function definitions included: as long as main.cpp does know about the type information from the Cube function signatures in Cube.h, the main.o file will be "**linked against"** the compiled definitions in the Cube.o file
# Standart library
The C++ standard library (std) provides a set of commonly used functionality and data structures to build upon.
### iostream
The iostream header includes operations for reading/writing to files and the console itself, including std: : cout.
```cpp
#include <iostream>

int main() {
	std:: cout << "Hello, world!" << std::endl;
	return 0;
}
```
### Namespace
All functionality used from the standard library will be part of the std namespace.
- Namespaces allow us to avoid name conflicts for commonly used names.
If a feature from a namespace is used often, it can be imported into the global space with using:
``` cpp
using std::cout;
```
For defining namespace we should put all inside it (same for .h and .cpp)
``` cpp
namespace uiuc {
	class Cube {
		public:
			double getVolume () :
			double getSurfaceArea () ;
			void setLength (double length) ;
			
		private:
		double length_;
	};
}
// usage
int main() {
	uiuc::Cube c;
}
```

# Memory
## A Variable's Memory Address 
In C++, the & operator returns the memory address of a variable.
## Stack Memory
By default, every variable in C++ is placed in stack memory.
Stack memory is associated with the current function and the memory's lifecycle is tied to the function:

- When the function returns or ends, the stack memory of that function is released.
- Stack memory always starts from high addresses and grows down:
- Stack memory deallocated as we leave the function
## Pointer
A pointer is a variable that stores the memory address of the data.
- Simply put: pointers are a level of indirection from the data.
In C++, a pointer is defined by adding an * to the type of the variable.
```cpp 
int * p = &num;
```
Dereference
Given a pointer, a level of indirection can be removed with the dereference operator * 
``` cpp
int num = 7;
int * p = &num;
int value_in _num = *p;
*p = 42;
```

## Heap
Heap memory allows us to create memory independent of the lifecycle of a function.
If memory needs to exist for longer than the lifecycle of the function, we must use heap memory.
- The only way to create heap memory in C++ is with the new operator.
- The new operator returns a pointer to the memory storing the data - not an instance of the data itself.
- The new operator in C++ will always do three things:
	1. ﻿﻿﻿Allocate memory on the heap for the data structure
	2. ﻿﻿﻿Initialize the data structure
	3. ﻿﻿﻿Return a pointer to the start of the data structure
- The memory is only ever reclaimed by the system when the pointer is passed to the delete operator.

- Memory to store an integer pointer on the stack
- ﻿﻿Memory to store an integer on the heap
``` cpp
 int * numPtr = new int;
```
The C++ keyword nullptr is a pointer that points to the memory address Oxo.

- ﻿﻿nullptr represents a pointer to "nowhere"
- ﻿﻿Address Ox0 is reserved and never used by the system.
- ﻿﻿Address Ox0 will always generate an "segmentation fault" when accessed.
- ﻿﻿Calls to delete Ox0 are ignored.


``` cpp
if (condition) {
	// true case
}
else {
	// false case
}
```

# Constructor
When an instance of a class is created, the class constructor sets up the initial state of the object.

## Copy constructor
``` cpp
#include "Cube.h"
#include ‹iostream>

namespace uiuc {
	Cube: : Cube() {
		length_ = 1;
	}
	
	Cube::Cube(const Cube & obj) {
		length_ = obj.length_;
	}
...
```

``` cpp
#include "../Cube.h"

using uiuc:: Cube;

void foo (Cube cube) {  //copy constructor called
	// Nothing :)
}

int main(){
	Cube c; //default constructor called
	
	foo(c);
	
	return 0;
}

Cube foo(){
	Cube c; //default constructor called
	return c;  //copy constructor called
}

int main() {
	Cube c2 = foo();
	return 0;
}
```

## Assignment operator
A copy constructor creates a new object (constructor).

An assignment operator assigns a value to an existing object.
- An assignment operator is always called on an object that has already been constructed.
- The automatic(default) assignment operator will copy the contents of all member variables.
``` cpp
Cube & Cube:: operator=(const Cube & obj) {
	length_ = obj. length_;
	std:: cout «< "Assignment operator invoked!" « std:: endl;
	return *this;
}
```

# Instances
In C++, an instance of a variable can be stored 
- directly in memory
	- By default, variables are stored directly in memory.
	- ﻿﻿The type of a variable has no modifiers.
	- ﻿﻿The object takes up exactly its size in memory.
- accessed by pointer
	- The type of a variable is modified with an asterisk (*).
	- ﻿﻿A pointer takes a "memory address width" of memory (ex: 64 bits on a 64-bit system).
	- ﻿﻿The pointer "points" to the allocated space of the object.
- accessed by reference
	- A reference is an alias to existing memory and is denoted in the type with an ampersand (&).
	- ﻿﻿A reference does not store memory itself, it is only an alias to another variable.
	- ﻿﻿The alias must be assigned when the variable is initialized.

### Passing by
Identical to storage, arguments can be passed to functions in three different ways:
- ﻿﻿Pass by value (default)
- ﻿﻿Pass by pointer (modified with *)
- ﻿﻿Pass by reference (modified with &, acts as an alias)
### Destructor
The only action of the automatic default destructor is to call the default destructor of all member objects.
An destructor should never be called directly. Instead, it is automatically called when the object's memory is being reclaimed by the system:
- ﻿﻿If the object is on the stack, when the function returns
- ﻿﻿If the object is on the heap, when delete is used

To add custom behavior to the end-of-life of the function, a custom destructor can be defined as:
- ﻿﻿A custom destructor is a member function.
- ﻿﻿The function's destructor is the name of the class, preceded by a tilde ~.
- ﻿﻿All destructors have zero arguments and no return type.

``` cpp
Cube: : ~Cube() {
	cout << "Destroyed $" << getVolume() << endl;
}
```
Note that using "delete" on a pointer frees the heap memory allocated at that address. However, deleting the pointer does not change the pointer value itself to "nullptr" automatically; you should do that yourself after using "delete", as a safety precaution. For example:
``` cpp
// Allocate an integer on the heap:
int* x = new int;

// Now x holds some memory address to a valid integer.
// Do some kind of work with the integer.
// We'll just set that integer to 7:
*x = 7;

// Now delete the pointer to deallocate the heap memory:
delete x;

// This destroys the integer on the heap and frees the memory.
// But now x still holds the memory address!
// Set x to nullptr for safety:
x = nullptr;
```
# Errors
``` cpp
// This code compiles successfully, runs, and CRASHES with a segfault!
int* n = nullptr;
std::cout << *n << std::endl;
```

# Templated types
A template type is a special type that can take on different types when the type is initialized. std::vector uses a template type:
``` cpp
#include <vector>
#include <iostream>

int main () {
	std:: vector<int› v;
	
	v. push_back(2);
	v. push_back(3);
	v. push_back(5);
	
	std:: cout << v[0] << std::endl;
	std:: cout << v[1] << std::endl;
	std:: cout << v[2] << std::endl;

	return 0;
}
```
A template variable is defined by declaring it before the beginning of a class or function:
``` cpp
template <typename T>
class List{
	private: 
	T data_;
}

template <typename T>
int max(T a, T b) {
	if (a › b) {
		return a; 
	} 
	return b;
}

```

# Inheritance
``` cpp
namespace uiuc {
	class Cube : public Shape {
		public:
			Cube(double width, uiuc::HSLAPixel color); 
			double getVolume() const;
			
		private:
			uiuc: :HSLAPixel color_;
	}
}
```

Initialization list available for constructor
The syntax to initialize the base class is called the initializer list and can be used for several purposes:

- ﻿﻿Initialize a base class
- ﻿﻿Initialize the current class using another constructor
- ﻿﻿Initialize the default values of member variables
``` cpp
Cube:: Cube(double width, uiuc::HSLAPixel color) : Shape(width) {
	color_ = color;
}
```