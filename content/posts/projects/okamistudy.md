+++
title = "Celestial Brush of Okami"
date = 2023-12-17T12:33:31+04:00
draft = false
categories = ['Projects']
tags = ['unity', 'csharp']
description = "Recreation of Okami's core gameplay mechanic."
cover = 'img/study/okami_640.gif'
+++

Developed by Clover Studio and directed by legendary Hideki Kamiya, Okami is among my favorite action adventure games ever, it's brilliant.

And upon discovering this [Mix and Jam Youtube video](https://www.youtube.com/watch?v=yuQXeaYBuuM), recreating Okami's brush mechanic, I got inspired to try and do so myself as well. 
By the way, [Mix and Jam channel](https://www.youtube.com/@mixandjam) is fantastic, full of really impressive gameplay recreations.

{{< figure src="img/study/okami_640.gif" alt="Okami Study" position="center" style="border-radius: 8px;" caption="Okami Study [github](https://github.com/TheCHead/OkamiStudy)" captionPosition="center" >}}

So, the project itself turned out to be quite challenging but immensely satisfying when it all worked out. Here are a couple of cool accomplishments from the project:
- seamless projection of the camera image onto the drawing canvas with a custom shader made with ShaderGraph
- a canvas drawing system featuring stylized brush stroke textures and fluid brush animations
- a brush stroke recognition system, utilizing [$P Point-Cloud Recognizer solution for Unity](https://github.com/DaVikingCode/PDollar-Unity)
- interpretation of a brush stroke gesture and its projection into the game world, resulting in actions like a tree-cutting slash attack or a cherry bomb

{{< youtube GYJXbgwV8Wk >}}
