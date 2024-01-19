+++
title = 'George & Apple Pie'
date = 2023-12-21T14:35:01+04:00
draft = false
categories = ['Projects']
tags = ['unity', 'devtools']
description = "Narrative adventure, currently in development."
cover = 'img/george/georgetitlem.png'
+++

Over the course of 2023, I participated in my colleague's indie project, George & Apple Pie. This is a story driven game with a heavy focus on hand-drawn animations.

Some notable features that I've developed include:
- Responsive 2D character controller, packed with a rather complex animation system, jump input buffers, invincibility frames, pickups, and all the good stuff.
{{< figure src="img/george/georgecontroller.gif" alt="George" position="center" style="border-radius: 8px" >}}
- Dialogue Graph Editor tool, built with Unity UI Toolkit and utilizing node-based structure.  
The tool allows building dialogue graphs, chaining and grouping nodes, saving and loading created graphs, plus a couple of convenience features, such as clearing or resetting the graph field.
{{< figure src="img/george/dialoguegraph.png" alt="Dialogue Graph" position="center" style="border-radius: 8px" caption="Dialogue Graph Editor [github](https://github.com/TheCHead/DialogueGraphTool)" captionPosition="center" >}}
The tool also comes with the Dialogue Inspector component to attach to game objects, making searching for needed nodes fast and easy.
{{< figure src="img/george/nodefinder.png" alt="Dialogue Finder" position="center" style="border-radius: 8px" >}}
- In-game dialogue UI system, typewriting character dialogues with the option of applying different visual effects to the text.
- Cutscene editor, made of Unity Timeline tool and custom timeline tracks and clips.
{{< figure src="img/george/customtimeline.png" alt="Custom timeline" position="center" style="border-radius: 8px" >}}