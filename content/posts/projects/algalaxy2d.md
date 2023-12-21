+++
title = 'Al Galaxy 2D'
date = 2023-12-21T15:00:00+04:00
draft = false
categories = ['Projects']
tags = ['skyeng', 'unity', 'ecs']
description = "Flagship reimagined."
cover = 'img/skyeng/galaxy2d.gif'
+++

In early 2023 we made a decision to transition [Al Galaxy]({{< ref "algalaxy" >}}) from 3D to 2D, so we could speed up our production pipeline and improve scalability of the project, as well as learn from mistakes and shortcomings of the initial game.

The Galaxy reimagination would differ in features, details and appearance, so it would become a full on remake, or rather reimagining, of the original.

{{< youtube etwK6IaRnG0 >}}

My role here was to build a scalable architecture for the new project, implementing all the sandbox gameplay features, plus developing some mini-games and operational features, like save system and analytics.

As of the project architecture, we chose an Entity Component System framework. It came to be the perfect fit, as the game became really easy to maintain and expand, making update release window much shorter. ECS also allows the code to be much more decoupled and modular just by the design of it, so I'm an official fan of the approach now. This time we would also rely on the use of Dependency Injection containers, which certainly made our life easier.

{{< figure src="img/skyeng/galaxy2d.gif" alt="Sandbox" position="center" style="border-radius: 8px;"  >}}

The game is also built as a platform for us to measure the impact that games can make on educational process and cognitive abilities of children, and we've already gained some valuable insight on the topic.

Al Galaxy is now available on both [iOS](https://apps.apple.com/ru/app/al-galaxy/id1626464795) and [Android](https://play.google.com/store/apps/details?id=com.Skyeng.AlGalaxy&hl=en_US&pli=1) platforms. Since its release, the player base has been steadily expanding, accompanied by positive retention and playtime metrics.