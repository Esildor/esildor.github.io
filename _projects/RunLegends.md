---
title: 'Run Legends'
subtitle: 'Turn walking and running into a cooperative multiplayer battle game!'
date: 2022-07-09 00:00:00
featured_image: '/images/projects/runlegends/banner.png'
---

## Learnings

Working at Talofa Games has taught me so much more about the game development process than I ever thought I could learn. It continues to play a pivotal role in how I grow as a developer and leader.

At other companies, my engineering skills improved and changed as I became a stronger and more confident contributor. This growth mainly came from writing gameplay systems from design specs. However, at Talofa, being the **Lead Client Engineer**, not only do my engineering skills continue to grow but so do my project management and leadership skills. Managing two other engineers and balancing their workload has proven to be a challenging, yet rewarding experience. I learned to love the humanity of it all. On a technical note, watching how other engineers use, modify, and extend my code is also one of the many valuable things.

---

## My Impact

On Run Legends, I and the Lead Server Engineer play essential roles in architecting the project from an empty Unity project. This *"day one"* level of intimacy with the project has been a treat and trip! Together we design and implement the majority of systems by hand. Here are a few:

- A purely ScriptableObject based content loading system, where designers can edit the game's configuration and content and deploy it to players using Unity's addressable system. Allow them to tweak gameplay on the fly!
- Integrated authentication services, allowing players to login and sync their accounts with multiple services. (Google Play Games, GameCenter, etc.)
- Created a real-time battle system that can run on the game server or locally on the client, all via a shared client/server library. This allows all the developers to share systems and code.

Additionally, purely on the client, I played critical roles or entirely engineered large systems for the game:

- Using the [XNode framework](https://github.com/Siccity/xNode), engineered a visual state machine that allows our audio engineers to build dynamic content for runs, all updatable via Addressables. Allowing the game to be patched and expanded without requiring new app updates.
- Built a mobile-friendly, stack-based UI paneling system that makes it easy to control the game's UI state.
- Integrated 3rd-Party systems like [PlayFab](https://playfab.com/) and [LunarConsole](https://github.com/SpaceMadness/lunar-unity-console), which were researched and vetted as a good pick for our project
- Created location plugins for iOS and Android to track the player's location and handle that data from inside of Unity.

---

## The Game

Run Legends is hands down one of the most unique and innovative projects I've ever worked on. For starters, the core game loop is a purely audio-driven experience. Players run or walk outside, using their phones GPS and their speed to control the game. Players run with other real players in real-time, and battle monsters with their weapons of choice, these weapons are catered to their run style. I know, it's ambitious, crazy, intriguing, and challenging and that's why it's such a treat to work on.

### Gameplay Images

Check out some images of the game!

<div class="gallery" data-columns="3">
<img src="/images/projects/runlegends/showcase-1.png">
<img src="/images/projects/runlegends/showcase-2.png">
<img src="/images/projects/runlegends/showcase-3.png">
<img src="/images/projects/runlegends/showcase-4.png">
</div>

---

## Pretty cool, huh?

Try it on your mobile device!

<a href="[https://www.talofagames.com/](https://www.talofagames.com/)" class="button button--large">(Coming Soon! Check out the Splash Page!)</a>