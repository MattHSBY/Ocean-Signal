Dependency Injection is a design pattern used in object-oriented programming to make code more modular, testable, and maintainable.
- Normally, classes create their own dependencies:
```C#
public class Car {
	private readonly Engine _engine;
	public Car() {
		_engine = new Engine();
	}
}
```
Car is hard-wired to use Engine here, and if you wanted to replace `Engine` with something like `ElectricEngine`, you'd have to modify the `Car` class.
- With Dependency Injection:
  ```C#
public class Car {
	private readonly IEngine _engine;
	
	public Car(IEngine engine) {
		_engine = engine;
	}
}
  ```
Here, `Car` doesn't care what engine it has, it just needs something that implements IEngine.


## How is dependency injection typically done?
### 1. Constructor injection
Dependencies are passed into the constructor.
```C#
var car = new Car(new ElectrincEngine());
```
### 2. Property injection
Dependencies are set via properties.
```C#
car.Engine = new ElectricEngine();
```
### 3. Method injection
Dependencies are passed directly into methods.
```C#
car.Start(new ElectricEngine());
```

## Ninject
Dependency injection can be done by using [[Ninject]]