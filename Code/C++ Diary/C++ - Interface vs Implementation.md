# File `.h` acts like both an Interface and Abstract Class in Java. 
C++ has file `.h` (Interface/Contract) and `.cpp` (Implementation).

## 1. How they work together 
To implement an interface, the `.cpp` file needs to `#include "[name_file].h"` at the very beginning. 
* *Note:* This is the exact same concept as `class Something implements SomeInterface` in Java.
## 2. File ".h" example:

```cpp 
#pragma once // Must have for every .h file - ensures it only compiles once 
struct Variable { 
	QString name; // public by default 
	int age; 
private: 
	QString email; // Can force private inside a struct, but rarely used 
}; // <-- MUST HAVE semicolon here! 
class Children : public Parent // class Children extends Parent (Inheritance is optional) 
{ 
	QString secret; // private by default 
public: 
	QString schoolName; //force public
	
	void study(); // Interface method - only declare the "shell", no logic here! 
}; // <-- MUST HAVE semicolon here!
```
## 3. File ".cpp" example:
```cpp
#include "child.h" // Must include the header file to connect the contract
#include <iostream>

// Use Scope Resolution Operator (::) to define the class "citizenship"
void Children::study() // must use :: to let the compiler know this method is in class Children that have been declared in .h
{
    std::cout << "I am studying"; // Define the logic that was declared in .h
}

```
## Note: For every methods were declared in .h file must be defined the logic inside .cpp file