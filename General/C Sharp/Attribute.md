Attributes provide a powerful way to associate metadata, or declarative information, with code (assemblies, types, methods, properties, and so on). After you associate an attribute with a program entity, you can query the attribute at run time by using a technique called reflection.

My most commonly used attribute is `[CallerMemberName]` which can be used like this:
```C#
protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null) 
{
	PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
// this way, when a property which wants to invoke this method in its setter calls the method, the [CallerMemberName] attributes autmoatically fills in propertyName based on the property that calls it, for example.

private int counter;
public int Counter 
{
	get => counter;
	set 
	{
		OnPropertyChanged();
		counter = value;
	}
}


```
