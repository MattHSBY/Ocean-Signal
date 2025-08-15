A value type is a kind of [[Type]] which stores its exact value, as an inherent characteristic, these are always copied "by value". These might include: `int`, `double`, `bool`, `struct`, etc... When a value type (like a struct) is a field inside a class, it lives on the heap, because the class instance itself is allocated on the heap. Whereas when a value type is a local variable, it is allocated within a [[StackFrame]] on the stack.

There are some instances where a variable looks like a local variable, but the compiler will see it differently and will therefore allocate it onto the heap.

For example:
```C#
int x = 0;
int y = x;

x= x+3
Console.WriteLine(y); // Output: "0".
```
In this example, even though y is equal to x, when x is changed, y doesn't change. This is because x just stores the value 0, and when y is assigned the value of x, it just stores the value 0 again, there is no tie to the specific object (reference), it just stores the value of x at that time (0).
Had x and y been [[Reference Type]]s, when changes happened to x, changes would also happen to y, because they would reference the same object under a different identifier.


Interesting article:
https://learn.microsoft.com/en-us/archive/blogs/ericlippert/the-stack-is-an-implementation-detail-part-one