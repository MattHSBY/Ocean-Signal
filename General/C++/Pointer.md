A pointer is a variable that stores the memory address of another variable as its value. 
```CPP
string food = "Pizza";
string* ptr = &food;

cout << food << "\n";
cout << &food << "\n";
cout << ptr << "\n";

// Output:
// Pizza
// 0x6dfed4
// 0x6dfed4


```
It has to be the same type as the variable it stores (so a string variable's pointer has to be declared a string). You can also "dereference" a pointer using the (\*) operator. This means to get the original value by visiting that memory address.
```CPP
string food = "Pizza";
string* ptr = &food;

cout << ptr << "\n";
cout << *ptr << "\n";

// Output:
// 0x6dfed4
// Pizza
```
