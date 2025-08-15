A reference type is a kind of [[Type]] which stores a reference (or address) to the actual data, rather than the data itself. These might include: Classes, Interfaces, Arrays, [[Delegate]]s, Strings, [[Record]]s.

This means that when you assign a reference type variable to another, both variables refer to the same actual object in memory. A reference type is stored on the [[Heap]]. 
For example:

```C#
class Car {
	public string NumberPlate;
}
Car A = new Car();
A.NumberPlate = "ABCDEF";

Car B = A;
B.NumberPlate = "GHIJKL";

Console.WriteLine(A.NumberPlate); // Output: "GHIJKL".
```
In this example, only B's number plate is changed, but because Car is a reference type (class) when A is assigned to Car B, Car B is actually assigned Car A's reference, as in, the exact same object, meaning any changes applied to A, apply to B, and vice versa. This kind of type is as opposed to [[Value Type]].

## String comparison question
An important question is if strings are reference types, how are we able to use the `==`/`!=` operators to compare their values when if we use these operators on other reference types their *references* are compared:
```C#
class myclass
{
//example empty class

}
void()
{
	myclass x = new myclass();
	myclass y = new myclass();
	Console.WriteLine(x==y); 
	// output: false, references compared, different objects, different references, not equal. 

}
```
But with strings:
```C#
void()
{
	string x = "x";
	string y = "y";
	Console.WriteLine(x==y);
	// output: true, but references are definitely different, so why?
}
```
In System.String's implementation there is:
```C#
public static bool operator ==(string? a, string? b) => string.Equals(a, b);

public static bool operator !=(string? a, string? b) => !string.Equals(a, b);

public bool Equals([NotNullWhen(true)] string? value)
{
  if ((object) this == (object) value)
    return true;
  return value != null && this.Length == value.Length && string.EqualsHelper(this, value);
}
```
 This is C#'s way of overloading the `==` operator, meaning that when we use `==` the default behaviour is overridden and the Equals method is used instead, which does actually compare the string's value.
