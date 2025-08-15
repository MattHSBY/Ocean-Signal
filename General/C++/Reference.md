A reference variable is a "reference" to an existing variable, and it is created with the & operator:
```C++
string food = "Pizza"; // variable food
string &meal = food;   // reference to variable food
```
If we were to do anything to the reference variable, it would do the same to the original variable too.
```CPP
string food = "Pizza";
string &meal = food;

meal = "Burger";

cout << food << "\n";
cout << meal << "\n";

// Output:
// Burger
// Burger

```
The same operator can also be used to get the memory address of a variable:
```CPP
string food = "Pizza";
cout << &food; // outputs 0x6dfed4
```
A variable which stores this memory address of another variable is called a [[Pointer]].