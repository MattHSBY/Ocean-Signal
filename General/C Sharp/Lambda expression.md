Lambda expressions are a way to concisely define anonymous methods.
```C#
(parameters) => expression_or_statement_block
```
For example:
```C#
x => x * x // x is a single parameter (even though it isn't in brackets)
(x, y) => x + y // 2 parameters
() => Console.WriteLine("Hello!"); // 0 parameters
```
Lambda expressions can be used to create [[General/C Sharp/Delegate.md|delegates]] anonymously.
```C#
public class Program
{
	public static void Main()
	{
		DoStuff((string text) => Console.WriteLine(text));
	}
	
	public static void DoStuff(Action<string> action)
	{
		action.Invoke("Hello World!");
	}
	
}
```
In the Main method, the DoStuff method is called, and the argument provided is an [[Action]], although a new [[Action]] instance is not instantiated. This is because the lambda expression `(string text) => Console.WriteLine(text)` is compiled into a [[Delegate]], more specifically an [[Action]] instance with a string parameter at runtime. 