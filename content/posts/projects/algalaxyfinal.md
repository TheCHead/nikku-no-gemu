+++
title = 'Al Galaxy Final Update'
date = 2025-01-14T17:46:06+04:00
draft = false
categories = ['Projects']
tags = ['skyeng', 'unity', 'ecs']
description = "Time to move on."
cover = 'img/skyeng/zoe_play.gif'
+++

By the end of 2024, we released the final content update for Al Galaxy, the game I've been working on for the past 3 years.
Below is a brief summary of what new features were added to the game and where it stands as I leave it.

{{< figure src="img/skyeng/toy_chest.png" alt="New content" position="center" style="border-radius: 8px;" >}}

### New Rooms
We developed two new rooms for players to explore: the Bathroom and the Farm. Each with it's own mechanics and secrets, offering a lot to discover and experiment with. 

{{< figure src="img/skyeng/bathroom_bedroom.png" alt="Bathroom and bedroom" position="center" style="border-radius: 8px;" >}}

{{< figure src="img/skyeng/farm.jpg" alt="Farm" position="center" style="border-radius: 8px;" >}}

My role was to develop all the player interactions within these sandbox environments and integrate the new assets created by the art team. I particularly enjoyed working on the gardening section - I think it turned out great.

{{< figure src="img/skyeng/farm_pot.gif" alt="Gardening" position="center" style="border-radius: 8px;" >}}

### New Mini-Games and NPC Events
Over the course of Galaxy development, we experemented a lot with incorporating educational content into the core gameplay loop. One such iteration was NPC events: after playing the game for some time, game characters would receive a phone call from either a forest druit or a robot, inviting players to visit their planet and help them solve a set of puzzles. 

{{< figure src="img/skyeng/npc_call.gif" alt="NPC Call" position="center" style="border-radius: 8px;" >}}

The druid's planet featured math challenges, while the robot's planet offered logic puzzles. Completing these mini-games rewarded players with sandbox items, new farm seeds, character clothes, accessories, and more.

Later though, NPC call system would be scrapped in favor of daily activities.
My contributions included developing the NPC call system, a few mini-games for it and a bunch of various reward items.

### Galaxy as Homework
Throughout most of 2024, we worked on making Al Galaxy a part of an educational program for preschoolers at Skysmart. The idea was that kids would study online with a teacher and then, as part of their homework, get a personalized set of mini-games in Al Galaxy based on their progress and skill level.

{{< figure src="img/skyeng/quiz_rush.gif" alt="Quiz Rush" position="center" style="border-radius: 8px;" >}}

This was realized through daily activities in the game. Each day, players receive a notification that new training is available. If choose to engage, they are offered a set of educational mini-games to play. Upon completing the session players can choose their reward - either a sandbox item or a resource to spend in the in-game shop.

For this feature, I developed over 10 educational mini-games (some based on earlier experiments) and the daily training reward system. I also configured 120+ unique completion rewards to unlock and play with.

{{< figure src="img/skyeng/bug_alarm.gif" alt="Bug Alarm" position="center" style="border-radius: 8px;" caption="Identify shapes and colors" captionPosition="center">}}

{{< figure src="img/skyeng/abc_tetris.gif" alt="ABC Tetris" position="center" style="border-radius: 8px;" caption="Find a letter" captionPosition="center">}}

### Shop and Storage
Along with making daily activities, we've introduced item chests - one in each room. These would serve as meta storage for all game items, as well as a shop (with no microtransactions). Players earn stars by completing mini-games and can spend them in the shop on all kinds of stuff.
It was up to me to develop item box systems and interfaces, setup the save system for it, and tie it with the daily reward system.

{{< figure src="img/skyeng/galaxy_box.gif" alt="Item Box" position="center" style="border-radius: 8px;" >}}

### Coloring Book
The last thing I worked on was the Coloring Book asset integration. Nothing really notable here, except asset shaders did not work in our project at first - I had to rewrite them for URP. After the issue was cleared, the feature came together beautifully thanks to the outstanding work of our artists on the linework and UI. It's full of content too, as new pictures and painting tools are unlockable through daily rewards as well.

{{< figure src="img/skyeng/coloring_book.jpg" alt="Item Box" position="center" style="border-radius: 8px;" >}}

### Results
To sum up my time with Al Galaxy, here's what I did for the project:
- Developed the first playable prototype of the 3D version of the game and then the MVP.
- Collaborated with third-parties to create 3D assets and animations for characters and environments. Worked on IK character animations and character skins.
- Integrated Zenject DI into the project.
- Optimized the game to run well on low-end devices.
- Supported and extended 3D Galaxy for a year, adding new rooms, items, activities, mini-games, parental control features, localization, and more.
- Rebuilt 2D version of the game using ECS architecture.
- Developed all sandbox systems of the reboot: character, item and environment interactions, positioning and save systems, UI and others - 100+ interconnected systems in total.
- Integrated Spine animations into the project. Setup layered character customization.
- Configured Addressabled and asset bundle cloud delivery.
- Built 10+ educational mini-games on math, English and logic.
- Created the daily training reward system and all associated rewards.
- Developed an in-game shop and item storage system.
- Integrated a coloring book mechanic into the game.
- Worked on analitics events.
- Published the game on both the AppStore and Google Play.
- Polished the game as much as I could.

And here's some game performance data:
- Median session length: 15+ minutes.
- Retention rates (day 1-7-30): 25%-10%-5%.
- Average sessions per daily active user: 2+.
- The release of Al Galaxy and subsequent content updates helped reduce the refund rate for Skysmart’s preschool products from 15% to 7%.
- Students engaged in educational activities in the game during every second session.
- I personally saw how much fun kids had playing our little game, which is the most important achievement for me.

As of now, you can still download Al Galaxy on [AppStore](https://apps.apple.com/ru/app/al-galaxy/id1626464795) and [Google Play](https://play.google.com/store/apps/details?id=com.Skyeng.AlGalaxy&pli=1).

### Bye Bye Galaxy
Well, that's about it. I've been part of Al Galaxy from its first brainstorm session to the last content update, and I'm grateful to have had such a wonderful team of people around me during all this time. Now, it's time to move on!

{{< figure src="img/skyeng/galaxy_costumes.png" alt="Goodbye!" position="center" style="border-radius: 8px;" >}}