## What is Ninject?
Ninject is a [[Dependency Injection]] (DI) framework for .NET. It acts as a container that manages dependencies for you.
Instead of manually writing up dependencies, you tell Ninject how to bind interfaces to implementations, and it will automatically provide the right objects when needed.
## Examples
### 1. (DI without  Ninject)
```C# 
public interface IEngine {
	void Start();
}

public class GasEngine : IEngine {
	public void Start() => Console.WriteLine("Gas engine starting...");
}
public class ElectricEngine : IEngine {
	public void Start() => Console.WriteLine("Electric engine starting...");
}

public class Car {
	private readonly IEngine _engine;
	public Car(IEngine engine) {_engine = engine;}
	public void Drive() {
		_engine.Start();
		Console.WriteLine("Car is driving...");
	}
}

var car = new Car(new ElectricEngine());
car.Drive();

```
### 2. (DI with Ninject)
```C#
class Program {
	static void Main() {
		IKernel kernel = new StandardKernel();
		
		kernel.Bind<IEngine>().To<ElectricEngine>();
		
		var car = kernel.Get<Car>();
		
		car.Drive();
	}
}
```
## Composition root
The composition root is the one place in your application where you set up the object graph - i.e., where you wire together all your dependencies and configure the DI container.
- There should be exactly one composition root in an application (per application/domain boundary). 
- The composition root is typically located at the entry point of the application (e.g., `Main()` in a console app, `Global.asax`/ `Startup` in a web app, or `Program.cs` in ASP.NET Core).
- The composition root is where you tell Ninject how interfaces map to concrete implementations (bindings).
- The composition root resolves the top-level service(s) your app needs to start running, and everything else gets injected automatically down the chain.

## Automatic generation
```C#
class Program
{
	static void Main(string[] args) {
		IKernel kernel = new StandardKernel();
		kernel.Bind<IRepository>().To<SqlRepository>();
		kernel.Bind<IService>().To<MyService>();
		
		var app = kernel.Get<App>();
		
		app.Run();
	}
}
```
This means that for whatever dependency that App needs, for example, these will also be generated automatically in the same way they are generated in the line `var app = kernel.Get<App>();`, for example:
```C#
public class App
{
	private readonly IService _service;
	
	public App(IService service) {
		_service = service;
	}
	
	public void Run() {
		Console.WriteLine("App starting...");
		_service.PerformTask();
	}
	
}
```
Notice already how an `IService` was never instantiated in this code, we just "got" the App from Ninject from kernel: `var app = kernel.Get<App>` and it recognised that `App` depended on `IService` was required and it automatically generated it.
```C#
public interface IService {
	void PerformTask();
}

public class MyService : IService {
	private readonly IRepository _repository;
	
	public MyService(IRepository repository) {
		_repository = repository;
	}
	
	public void PerformTask() {
		Console.WriteLine("Service doing work...");
		var data = _repository.GetData();
		Console.WriteLine($"Data retrieved: {data}");
	}
	
}
```
`MyService` depends on `IRepository`
```C#
public interface IRepository {
	string GetData();
}

public class SqlRepository : IRepository {
	private readonly string _connectionString;
	
	public SqlRepository() {
		_connectionString = "Server=.;Database=MyDb;Trusted_Connection=True;";
	}
	
	public string GetData() {
		return "Hello from SQL Repository!";
	}
}
```
At no point in this example are any of these classes being "made" Ninject just automatically generates them:

### Flow
1. `Program.Main` asks Ninject for an `App`
2. Ninject sees that `App` needs an `IService` -> resolves `MyService`
3. `MyService` needs an `IRepository` -> resolves `SqlRepository`
4. `SqlRepository` is constructed (possibly with its own dependencies).
5. The fully wired `App` is returned.
