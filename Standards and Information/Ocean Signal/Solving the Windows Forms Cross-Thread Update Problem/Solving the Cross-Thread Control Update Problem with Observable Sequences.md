## Introduction

One of the common pitfalls of Windows Forms applications is that Windows Forms is not thread-safe, and all control updates must occur on the thread that created the control. In practice, this almost always means on the UI thread, which uses the Single-threaded Apartment (STA) threading model.

## What is a Single Threaded Apartment?

A Single Threaded Apartment, or _STA Thread_, is a thread that uses a mailbox to queue events, and a _message pump_ to process those events.

In order to make a cross-thread call to a method in an STA-thread, an event must be posted to the windows message queue on that STA thread.

## The Problem

Most applications are multi-threaded. If you receive data from a serial port, the `DataReceived` event likely happens on a different thread. If you handle a timer tick event, most likely that happens on another thread. Lots of things happen on threads,

However, to update any controls on a Windows Form, that update must happen on the UI thread. This results in the programmer having to _Marshal_ updates onto the UI thread and Microsoft has done very little to make this easy. Typically, the problem is solved like this:

```csharp
private void UpdateLabel()
    {
        // Update the label on the UI thread
        if (myLabel.InvokeRequired)
        {
            myLabel.Invoke(new Action(() => myLabel.Text = "Updated Text"));
        }
        else
        {
            myLabel.Text = "Updated Text";
        }
    }
```

The `Control.Invoke` method is used to marshal the method call to the UI thread.

This works, but the developer ends up having to focus on the details of thread synchronization and the marshalling of data between threads. This is extremely easy to get wrong. Multi-threading is one of the most difficult aspects of programming, even for very experienced developers and there are many traps for the unwary developer. 

## A Declarative Approach

When working in a high-level language like C#, whenever possible, it is better to use _declarative style_: tell the computer _what to do_ and not _how to do it_. A _declarative style_ is almost always preferable to an _imperative style_. This is sometimes an alien concept for some developers who are used to working at low-level, where it's all about the details and everything is imperative. In C#, LINQ (Language Integrated Query) is a great example of declarative style:

```csharp
var query = from beacon in items
	where beacon.ProductSerialNumber == productSerialNumber
	orderby beacon.UtcDateAdded descending
	select beacon;
```

## Reactive Programming: Push vs Pull

Reactive programming is a technique where a declarative style can be used to process sequences of data. This can lead to a kind of logic inversion, where instead of _waiting_ for the next piece of data, we _declare_ what we would like to happen if and when the data becomes available. In other words, we _react_ to things as they happen.

An `IEnumerable<T>` is a "pull" collection. We can process the data by asking for each value in turn, and doing something to it. This is usually done in an imperative style, often in a `foreach` loop.

An `IObservable<T>` is a "push" collection. It describes a sequence of data that hasn't arrived yet, and lets us declare what should happen if and when it does arrive.

And so we arrive at a possible approach for handling cross-thread control updates. We present our data as an `IObservable<T>`. We declare what should happen when the sequence produces a data element, and we declare that it should be processed on the UI thread.
## Reactive Extensions for .NET (RX.Net)

Microsoft's Reactive Extensions for .NET provide everything we need. If we have an observable sequence, then to update a control on a Windows Form, We can write something like this:
```csharp
        model.ObservableValues
            .ObserveOn(SynchronizationContext.Current)
            .Subscribe(item => label2.Text = item.ToString());
```

The above code is abstracted from the worked example below.

Things to note.
- The update query goes in the Windows Form, this is presentation logic.
- Production of the data sequence goes in the ViewModel, this is program "business logic".
- There is no coupling from the ViewModel to the View. The ViewModel knows nothing about the view. This is a best-practice and ensures that the ViewModel is testable without relying on any presentation logic.
## Worked Example

The following Windows Forms application consists of:

- A `MainView` that displays a value.
- A `MainViewModel` that generates values to be displayed. The values are generated on a Thread Pool thread and exposed as an `IObservable<int>` observable sequence.

Details fo the `MainView` are not shown here, but the form has two labels, `label1` contains static text, and `label2` that will display values from the observable sequence.
![[MainView.png.png]]

The program generates a series of integer values by running a generator method on a Thread Pool thread. These values are fed into a `Subject<int>` which is exposed to the main view as an `IObservable<int>`.

The `MainView` subscribes to the observable sequence in the `SubscribeOservables` method. This is the key part of the solution, and uses the `.ObserveOn` operator to ensure that items are observed (processed) via the UI thread's `SynchronizationContext`.
```csharp
private void SubscribeObservables() =>
        model.ObservableValues
            .ObserveOn(SynchronizationContext.Current)
            .Subscribe(item => label2.Text = item.ToString());
```
### Program.cs

```csharp
namespace RxDemo;

internal static class Program
{
    /// <summary>
    ///     The main entry point for the application.
    /// </summary>
    [STAThread]
    private static void Main()
    {
        // To customize application configuration such as set high DPI settings or default font,
        // see https://aka.ms/applicationconfiguration.
        ApplicationConfiguration.Initialize();
        var viewModel = new MainViewModel();
        var view      = new MainView(viewModel);
        Application.Run(view);
    }
}
```
### MainView.cs

```csharp
using System.Reactive.Linq;

namespace RxDemo;

public partial class MainView : Form
{
    private readonly MainViewModel model;

    public MainView(MainViewModel model)
    {
        this.model = model;
        InitializeComponent();
        SubscribeObservables();
    }

    private void SubscribeObservables() =>
        model.ObservableValues
            .ObserveOn(SynchronizationContext.Current)
            .Subscribe(item => label2.Text = item.ToString());

    private void MainView_Load(object? sender, EventArgs e) => model.StartValueGeneratorOnThreadPool();
}
```


### MainViewModel.cs
```csharp
using System.Reactive.Linq;
using System.Reactive.Subjects;

namespace RxDemo;

public class MainViewModel
{
    private readonly Subject<int>     valueSequence = new();
    public           IObservable<int> ObservableValues => valueSequence.AsObservable();

    private void GenerateValues()
    {
        for (var i = 0; i < 1000; i++)
        {
            Thread.Sleep(1000);
            valueSequence.OnNext(i);
        }
    }

    public void StartValueGeneratorOnThreadPool() => Task.Run(GenerateValues).ConfigureAwait(false);
}
```