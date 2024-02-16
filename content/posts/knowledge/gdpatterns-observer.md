+++
title = 'GD Patterns: Observer'
date = 2024-02-15T18:01:37+04:00
draft = false
categories = ['Knowledge']
description = "Platinum Trophy Unlocked."
tags = ['unity', 'csharp', 'architecture']
+++

A continuation of the _GD Patterns_ series, following the _Game Programming Patterns_ [book](https://gameprogrammingpatterns.com) by Robert Nystrom, and applying its insights within the Unity environment.

## Observer

[Observer](https://gameprogrammingpatterns.com/observer.html), being a behavioral pattern, is one of the most popular and well-known design patterns from the Gang of Four. It has a native implementation in C# using the `event` keyword and in the Unity API using the `UnityEvent` class.

The Observer pattern is built around a subscription model, allowing the observed subject to notify its subscribers by dispatching events when certain criteria are met. Upon receiving the notification, subscribers (or observers) react accordingly, separating the event-handling logic from the subject."

### Example usage

So, the pattern consists of two entities: the observer and the subject being observed. Let's break down a basic subscription model.

First, let's start with the `Observer` class. At this stage, it only contains a method to be called upon notification by the subject.

```csharp
public class Observer
{
    public void OnNotify()
    {
        Debug.Log("Notification received.");
    }
}
```

Next, the `Subject` class. Its job is to keep track of observers (in this case, in a `List`) and notify them when something happens by invoking the `OnNotify()` method on each of them.

```csharp
public class Subject
{
    private List<Observer> _observers = new();

    private void Notify()
    {
        foreach (Observer observer in _observers)
        {
            observer.OnNotify();
        }
    }
}
```

The subscription bit comes into play with exposing a public API for adding and removing observers to the subject:

```csharp
public class Subject
{
    //...

    public void AddObserver(Observer observer)
    {
        _observers.Add(observer);
    }

    public void RemoveObserver(Observer observer)
    {
        _observers.Remove(observer);
    }
}
```

This setup can lead to a situation where the `Observer` is destroyed, but the `Subject` still keeps a reference to it. This situation can result in a `NullReferenceException` when invoking the event, which is big no-no.
The way to deal with this is to unsubscribe the observer from the subject right in the destructor. In the example below, I've also added a constructor to demonstrate subscribing and unsubscribing more clearly.

```csharp
public class Observer
{
    private Subject _subject;

    public Observer(Subject subject)
    {
        _subject = subject;
        _subject.AddObserver(this);
    }

    ~Observer()
    {
        _subject.RemoveObserver(this);
    }

    public void OnNotify()
    {
        Debug.Log("Notification received.");
    }
}
```

To sum up, here's a quote from Robert Nystrom's book:

    It’s an observer’s job to unregister itself from any subjects when it gets deleted.


### Making it better

Up to this point, our subscription system is very rigid and not very usable, as it's tightly coupled to specific classes.

We could make it more versatile by using interfaces for both the observer and subject classes, but even then, it wouldn't be very agile. In our case, making the observer system function-based and using delegates would be a more appropriate fit, as a `delegate` is essentially a reference to a method with a predefined signature.

First, we need to define a `delegate`, specifying its return type (`void`), name (`OnNotifyHandler`) and the list of arguments in the brackets (just a `Subject` class reference to keep it simple).

```csharp
public class Subject : MonoBehaviour
{
    public delegate void OnNotifyHandler(Subject subject);
}
```

Then we replace `Observer` references with `OnNotifyHandler` references everywhere within `Subject` class.

```csharp
public class Subject
{
    public delegate void OnNotifyHandler(Subject subject);

    private List<OnNotifyHandler> _observers = new();


    private void Notify()
    {
        foreach (OnNotifyHandler observer in _observers)
        {
            observer.Invoke(this);
        }
    }

    public void AddObserver(OnNotifyHandler observer)
    {
        _observers.Add(observer);
    }

    public void RemoveObserver(OnNotifyHandler observer)
    {
        _observers.Remove(observer);
    }
}
```

At last, we also need to modify the Observer a bit. 

```csharp
public class Observer
{
    private Subject _subject;

    public Observer(Subject subject)
    {
        _subject = subject;
        _subject.AddObserver(OnNotify);
    }

    ~Observer()
    {
        _subject?.RemoveObserver(OnNotify);
    }

    public void OnNotify(Subject subject)
    {
        Debug.Log($"Notification from {subject.name} received.");
    }
}
```

As seen in the constructor, we can now pass `OnNotify()` method of the `Observer` to the `Subject` directly. Now, the observer of any type can subscribe to the subject by providing a method of the right signature.

### Observer in C# and Unity

All of the above was examined to gain a better understanding of the Observer pattern's underlying mechanisms, but you most probably won't ever be writing it all yourself, as there are baked-in implementations of it pretty much everywhere.

Firstly, C# has its own implementation of the Observer pattern in the form of the `event` keyword, which simplifies things (and improves performance).

Let's see how the `Subject` would change when using `event`."

```csharp
public class Subject
{
    public event Action<Subject> OnNotify;

    private void Notify()
    {
        OnNotify.Invoke(this);
    }
}
```

As seen above, only the declaration of the event remains, which is invoked when needed. There's no need to keep track of observers or write a subscription API—the `event` keyword does it all for you. `Action` is a delegate with a `void` return type, and method arguments are encapsulated within `<>` brackets.

The `Observer` class remains largely unchanged, except for a slight change in the subscription syntax.

```csharp
public class Observer
{
    private Subject _subject;

    public Observer(Subject subject)
    {
        _subject = subject;
        _subject.OnNotify += OnNotify;
    }

    ~Observer()
    {
        _subject.OnNotify -= OnNotify;
    }

    public void OnNotify(Subject subject)
    {
        Debug.Log($"Notification {subject.name} received.");
    }
}
```

Unity has its own implementation of the Observer pattern—the `UnityEvent` class. Here's how it works.

```csharp
public class Subject
{
    public UnityEvent<Subject> OnNotify;

    private void Notify()
    {
        OnNotify.Invoke(this);
    }
}

public class Observer
{
    private Subject _subject;

    public Observer(Subject subject)
    {
        _subject = subject;
        _subject.OnNotify.AddListener(OnNotify);
    }

    ~Observer()
    {
        _subject.OnNotify.RemoveListener(OnNotify);
    }

    public void OnNotify(Subject subject)
    {
        Debug.Log($"Notification {subject.name} received.");
    }
}
```

Not much has changed:`event Action<Subject>` has turned into `UnityEvent<Subject>` in the `Subject`, and the subscription syntax has been altered in the `Observer` class.

So, the two primary options of utilizing the Observer pattern within Unity environment are `event` and `UnityEvent`. But when should we choose one over the other?

`UnityEvent` supports serialization in the Unity editor, allowing subscribers to be added or removed from outside the code. This makes it a useful tool for non-developers.

{{< figure src="img/knowledge/unityevent.png" alt="Enable GPU Instancing" position="center" style="border-radius: 8px;"  >}}

However, in other cases, there's no visible benefit to using `UnityEvent` over C# `event`, as Unity's implementation is at least 2 times slower and generates more garbage over time. For a detailed examination of the topic, please refer to the [article](https://www.jacksondunstan.com/articles/3335) by Jackson Dunstan.
