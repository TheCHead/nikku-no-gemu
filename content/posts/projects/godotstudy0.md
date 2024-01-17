+++
title = 'Godotstudy0'
date = 2024-01-12T17:22:25+04:00
draft = true
+++

- In Godot, a game is a tree of nodes that you group together into scenes. You can then wire these nodes so they can communicate using signals.
- Editor is all basic stuff: Scene, File browser, Inspector.
- Nodes are building blocks, use a search bar. Notes have signals to emit and subscribe to.
- Start with player character, CharacterBody2D node specifically, as it is not affected by physics at all, but interacts with other physics objects and is supposed to be controlled by the player. By default the node has a warning, that it requires collider to interact with other objects.
- New nodes with + sign, or ctrl+A (win), cmd+A (mac), ColliderShape2D.
- Godot nodes are saved in tscn forman (text scene), can be opened with text editor and is human readable. Basically anithing is stored as a scene, combining functions of both scenes and prefabs in Unity. Scenes can be nested, like prefabs.
- Scripts can be attached to a node, by default extending base class of the node. Cool feature is a built-in class reference. Either ctrl+click on class name in Script tab, or Help->Search help, or "doc" button in inspector right at the top.
- _physics_process is Update, with move_and_slide called at the end to apply change.
- Input Map is cool. Allows assigning different inputs to an alias, to then use in code.
- To call a method from another Node in tree there's a get_node() method, with a string parameter of a relative path to the needed node. A shirtcut to use is a $ sign, as $'NodeName'
- Another trick is that you can mark the node as a unique name node (% sign is added to the node icon). Now you can access the node anywhere in tree just by %'NodeName'. It's also performance-beneficial, as this way godot finds the node only once and then stores it in memory.
- ctrl+D for multiple selection in script.
- Built-in Y-sorting on CanvasItem node for it's children.
- Main scene (Start scene) is set in Project->Project Settings->Run.
- Annotationed function call, ex. @onready do smth is equall as calling method _ready()
- After the game start, there's a new Remote tab, where you can inspect all the nodes in game. Base node is called root.
- Marker2D node, same as basic Node2D, but with a cross marker. Convenient for non-entities, as pivot points or groups.
- Command+Shift+O to quickly search through all scenes.
- CollisionShape2D is a collider, Area2D is a trigger.
- Load object with load() and preload() functions. First is on demand, second on init. Need to call instantiate() to create an instance of the object.
- CanvasItem->Visibility->TopLevel bool to ignore render and transform inheritance.
- Timer node





