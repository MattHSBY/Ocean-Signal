The stack is a region of memory that stores [[Value Type]]s, method call [[StackFrame]]s and local variables. It operates in a Last-In, First-Out (LIFO) manner - meaning the last item pushed onto the stack is the first one to be popped off. The stack is a chunk of memory reserved by the operating system **per thread.** Each thread has its own separate stack, it grows/shrinks as methods are called and return, it lives in process memory but it is not a heap - it's a fixed size contiguous block. On windows, the default stack size is often 1 MB for .NET processes (can be changed).

It might be important to note that every object directly stored by the stack is a [[StackFrame]], however, [[Value Type]] variables which are stored in stack memory are stored inside each [[StackFrame]] as that [[StackFrame]]'s local variables.


## Stack Processing
![[Pasted image 20250815093455.png]]

For each thread, this is how methods are processed, methods are pushed onto the stack, and the thread is processing each method on the stack linearly, popping methods off once they have finished processing. Technically, in C# each method in the stack belongs to a "[[StackFrame]]". [[StackFrame]]s are created when a method (or function) is called, not for each line or code or instruction, so there is a difference when inside a method you have:
```C#
void Y()
{
	Console.WriteLine("Hello World!");
}

void X()
{
	int A = 10;
	int B = 5;
	A = A + B; // all of this so far simply belongs as part of X's StackFrame.
	
	Y(); // This is another method call, so at this point, there would be a new StackFrame pushed to the stack for Y().
}

X(); // Initiate X()

```
In this example, the call `Y()` creates a new [[StackFrame]] for this method and is pushed onto the stack.
![[Pasted image 20250815094715.png]]
So when Y() has finished processing, it is popped and the only thing left is X(), so then that can finish processing too, and then be popped off too.


## Stack overflow
A stack overflow occurs when a thread's call stack exceeds its allocated size.
- Each thread has a fixed-size stack (default ~1MB)
- Each method call adds a stack frame
- If too many frames are added - For example, from deep or infinite recursion (see below) - the stack pointer eventually runs past the reserved stack memory and the runtime throws a `StackOverflowException`
Here is an example:
```C#
void Recurse()
{
	Recurse();
}

Recurse(); // Initiate Recurse()
```
In the example, the `Recurse()` method is called and a StackFrame is created and pushed onto the Stack. As the first `Recurse()` method is processing, another method call is processed, causing a second [[StackFrame]] to be created for the method `Recurse()`, remember that the first [[StackFrame]] cannot get to the end until the second is finished, due to the nature of a Stack. 