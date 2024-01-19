+++
title = 'GD Patterns: Command'
date = 2024-01-17T18:08:06+04:00
draft = false
categories = ['Projects']
description = "Game Programming Patterns revisited."
tags = ['unity', 'csharp', 'architecture']
+++

This post is the first in a series where I revisit the _Game Programming Patterns_ [book](https://gameprogrammingpatterns.com) by Robert Nystrom. This time I will be reimplementing some of the patterns in Unity and C# to gain a better understanding of them and to keep it as a convenient reference source for myself.

Without further ado, the first pattern.

## Command

Robert gives the following tagline for the [Command](https://gameprogrammingpatterns.com/command.html) pattern:

    A command is a reified method call.

"_Reify_" means "_make real_" or, more casually, "_thingify_". Basically, a _Command_ is an action wrapped in an object; a verb turned into a noun.

### Example usage

Consider that we have an input handler, detecting player input and translating it into an action.

```csharp
public class InputHandler : MonoBehaviour
{
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.J)) Attack();
        else if (Input.GetKeyDown(KeyCode.K)) Jump();
        else if (Input.GetKeyDown(KeyCode.L)) Special();
        else if (Input.GetKeyDown(KeyCode.I)) Shoot();
    }

    // action implementations below
}
```

This setup hardwires the key input to a specific resulting action, which becomes problematic if we intend to swap button actions mid-game or allow players to remap controls.

To support that, we need to replace hardcoded button action with an abstraction—an object we could easily swap with an analogue in the future.

First, let's define an interface for a generic command with a single method.

```csharp
public interface ICommand
{
    public void Execute();
}
```
Then, we create a class implementation of the interface for each player action.

```csharp
public class AttackCommand : ICommand
{
    public void Execute()
    {
        Debug.Log("Attack");
    }
}

public class JumpCommand : ICommand
{
    public void Execute()
    {
        Debug.Log("Jump");
    }
}

public class ShootCommand : ICommand
{
    public void Execute()
    {
        Debug.Log("Shoot");
    }
}

public class SpecialCommand : ICommand
{
    public void Execute()
    {
        Debug.Log("Special");
    }
}
```

Now, within the InputHandler class, we define a list of commands for each button, and then assign an instance of a specific action to it.

By doing so, we eliminate tight coupling between input and action, enabling us to dynamically reassign button mappings.

```csharp
private ICommand _jCommand;
private ICommand _kCommand;
private ICommand _iCommand;
private ICommand _lCommand;

private void Awake() 
{
    _jCommand = new AttackCommand();
    _kCommand = new JumpCommand();
    _iCommand = new ShootCommand();
    _lCommand = new SpecialCommand();
}

void Update()
{
    if (Input.GetKeyDown(KeyCode.J)) _jCommand.Execute();
    else if (Input.GetKeyDown(KeyCode.K)) _kCommand.Execute();
    else if (Input.GetKeyDown(KeyCode.I)) _iCommand.Execute();
    else if (Input.GetKeyDown(KeyCode.L)) _lCommand.Execute();
}
```

But there's still room for improvement. This approach implies that each command already has everything it needs to do its job, which is rarely the case.

For example, to execute an action, we need an actor. Therefore, we can pass it as a parameter in the Execute() method.

```csharp
public interface ICommand
{
    public void Execute(IActor actor);
}
```
We need to update input handler accordingly.

```csharp
void Update()
{
    if (Input.GetKeyDown(KeyCode.J)) _jCommand.Execute(Actor);
    else if (Input.GetKeyDown(KeyCode.K)) _kCommand.Execute(Actor);
    else if (Input.GetKeyDown(KeyCode.I)) _iCommand.Execute(Actor);
    else if (Input.GetKeyDown(KeyCode.L)) _lCommand.Execute(Actor);
}
```

As for the Actor itself, we make it an implementation of the IActor interface, encapsulating input behaviour within the actor class. This approach allows for distinct input reactions for different actor classes.

```csharp
public interface IActor
{
    public void Attack();
    public void Jump();
    public void Shoot();
    public void Special();
}
```

We could push the idea further to make each actor subscribe to any command emitter (like the InputHandler class in the example above). This would enable players to control multiple actors simultaneously, allow actors' actions to be influenced by other sources, or even permit an arbitrary AI engine to dispatch commands to mobs in a similar manner.


### Command Stack
Another application of the Command pattern is that turning an action into an object, we can now store it and reuse it in the future.

Here, we introduce a stack to keep track of all performed actions. This time, we create a new command instance per player input, as the same actions can be performed by different actors.

```csharp
 private Stack<ICommand> _commandStack = new();

 void Update()
    {
        if (Input.GetKeyDown(KeyCode.J)) ExecuteCommand(_jCommand, Actor);
        else if (Input.GetKeyDown(KeyCode.K)) ExecuteCommand(_kCommand, Actor);
        else if (Input.GetKeyDown(KeyCode.I)) ExecuteCommand(_iCommand, Actor);
        else if (Input.GetKeyDown(KeyCode.L)) ExecuteCommand(_lCommand, Actor);
    }

    private void ExecuteCommand(ICommand command, IActor actor)
    {
        // create new instance of a command
        var commandType = command.GetType();
        var newCommand = (ICommand)Activator.CreateInstance(commandType);
        // execute command
        newCommand.Execute(actor);
        // push newly created command to stack
        _commandStack.Push((ICommand)newCommand);
    }

```

Now, we have a stack of actions performed by an actor, which we can use, for instance, in a playback mode. Alternatively, we could use a Queue instead of a Stack to buffer actions and execute them sequentially.

This structure allows us to easily implement the undo functionality. For this we need to extend the ICommand interface with an additional method, Undo(). Note that the ICommand interface is now also extended with an IActor property, enabling the Undo operation to be performed on the same actor.

```csharp
public interface ICommand
{
    IActor Actor { get; set; }

    public void Execute(IActor actor);

    public void Undo();
}
```

All that is left to do is to detect the undo input, pop the last action from the stack, and call the Undo() method.

```csharp
void Update()
{
    if (Input.GetKeyDown(KeyCode.J)) ExecuteCommand(_jCommand, Actor);
    else if (Input.GetKeyDown(KeyCode.K)) ExecuteCommand(_kCommand, Actor);
    else if (Input.GetKeyDown(KeyCode.I)) ExecuteCommand(_iCommand, Actor);
    else if (Input.GetKeyDown(KeyCode.L)) ExecuteCommand(_lCommand, Actor);

    // catch undo
    else if (Input.GetKeyDown(KeyCode.U)) UndoCommand();
}

private void UndoCommand()
{
    ICommand commandToUndo = _commandStack.Pop();
    commandToUndo.Undo();
}
```

The undo logic can be encapsulated within the actor class in the same way as a regular action (although it doesn't make much sense to undo attacking or jumping).

The Redo functionality can be done in a similar way. We just need to replace the Queue with a List and manage the index of the current action.


