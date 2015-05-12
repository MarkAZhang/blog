---
layout: post
title: "How I Fixed Up My Broken 3 Year Old Code Base - An HTML5 Game Post-Mortem" 
date: 2015-05-04 20:45
author: Mark Zhang
---

Code refactoring is about the least exciting thing you can do for two months when you're trying to launch a game. *Ain't nobody got time for that, amirite?*

But Impulse's three-year-old code base, which has gone through multiple design iterations, was seriously starting to offend my sensibilities.

Thus, I spent the majority of March and April paying down my technical debt. I'll go into what I did and what I learned.

### Brief Background

Impulse is a tough-as-nails top-down shooter crafted for hardcore players and written entirely in HTML5. Impulse is available online [here](http://play-impulse.com).

I work on Impulse in my off-time with one other friend. I am employed full-time as a front-end web developer.

### Cleaning up the code base

Here's what was wrong:

* **Dead code everywhere** - I have a bad habit of commenting out code instead of deleting it (even though I use version control!), especially if I'm not 100% sure about cutting a feature. 90% of the time I don't look back but I forget to delete the commented code.
* **Confusing, multi-purpose modules** - I have this class called MainGameSummaryState. Originally, it just showed end-of-level stats. But then I made it handle the UI when the player saved-and-quit the game. At the time, it was the easy thing to do, and besides, the two UIs were similar and both occurred when the player left the level. But the end result was a mess. There were multiple paths into MainGameSummaryState, each of which had to be configured with various optional variables. The internals were a tangle of if-statements.
* **Poorly organized code** - Sometimes it's obvious when you should bundle logic into a class--for example for each of Impulse's enemy types. Sometimes it's not. At first, the active game data for a play-through was so simple, a basic object seemed to suffice. But the complexity of the data grew and grew, and at each step, it seemed easier to keep adding logic to various game-states. The result was that the logic for the game data was spread out in an unintuitive way and very hard to recall about while developing.
* **Redundant code** - No matter how tempting it may be, never copy and paste logic from one file to another. Always take the time to factor it out. Otherwise, not only will your files get bogged down, but making changes will take longer because you have to change code in multiple places.

Overall, I found myself asking 'where does this happen in the code?' and 'what the hell does this even do?' far too often. Unexpected side-effects were also common. The time spent re-tracing through code and tracking down obscure bugs was substantial and made working on the game less fun.

Here's what I did to improve my code quality:

* **Removed all the dead code** - It's still all preserved in version control. I tried to remove the code in steps and be descriptive with my commit messages so I could easily find deleted code in the future. For code that I wanted to switch on and off (for example, an fps counter), I wrapped the code in a boolean flag.
* **Carefully split up my modules** - My rule of thumb was that every module should do exactly one thing. If two modules share code, factor it out into a common library or common class. I made sure I could succinctly describe what was in each utility library (and I had many). I went from about 40 files to 110, but it felt much more manageable due to the organization.
* **Organize related code into classes** - For example, I created a class to hold the gameplay data and defined an API for game-states to modify it. As a result, all the logic for processing the gameplay data was in one place instead of scattered across game-states. In doing this, I also discovered many ways to simplify the code and cut down on redundancies.


### A quick word on game engines

I really regret not using a game engine for a project as large as Impulse. It's fine to get your hands dirty building stuff from the ground up for a smaller project, but when you're doing something this big, you need all the help you can get, and game engines like [Unity](https://unity3d.com/) or [Phaser](https://phaser.io/) perform a lot of common tasks for you. One other hidden benefit is they naturally help you organize your code, since there's usually a 'right' way to plug into the engine or call its APIs.

**TLDR**: Research engines and frameworks when starting a project. You'll thank yourself later.

### Converting to node-style requires

I also decided to convert my code to use [node-style require syntax](https://github.com/substack/browserify-handbook#require). Basically, this allowed me to specify what dependencies each of my files had, and also what methods in each file would be exposed to other files. 

Benefits included:

* **Forced me to stomp out global variables**
* **Naturally improved organization of code**
* **Removed dependency graph issues** - I no longer had to worry about Javascript files loading out of order.
* **Access to node.js dev tools** - I was able to package Impulse up as a node.js package and use the many wonderful tools that the node.js community has developed. Some of the tools I've found particularly useful are:
  * **[Browserify](http://browserify.org/)** - automatically walks your dependency tree and compiles all your files into a single file. Previously, I had to explicitly list all my files.
  * **[Uglify](https://github.com/mishoo/UglifyJS)** - Javascript minification.
  * **[Gulp](http://gulpjs.com/)** - allows you to create an automatic build pipeline by combining technologies like the two above.
  * **[Lodash](https://lodash.com/docs)** - a Javascript utilies library that makes for cleaner code. No more for-loops!

### Converting to the PIXI Rendering engine

Finally, I decided to convert my game to use the [PIXI](http://www.pixijs.com/) Rendering Engine instead of making calls directly to the Canvas API. I was reaching the performance limit of Canvas, and there were a number of visual effects I still wanted to add. 

Pros of PIXI:

* **Performance** - PIXI provides a 2D-optimized WebGL Renderer in addition to the standard 2D one. The API is the same for both, allowing for graceful fallback.
* **Sprites are first-class objects** - Instead of rotating the screen to draw a rotated sprite every frame, you can just modify the rotate attribute on a sprite object and everything else is taken care of. This makes the code much cleaner.
* **Object-oriented style** - Allows for better code organization

Cons of PIXI:

* **Conversion pains** - Canvas 2D is more imperative while PIXI is object-oriented. Re-architecting Impulse to handle PIXI (i.e. re-writing ALL of my pre-existing rendering code) has been quite brain-draining and time-consuming.
* **Graphics are not first-class objects** - Unlike sprites, I can't save a Polygon object and pass it around or modify it. If I need a polygon to change every frame, I must make a Graphics object and re-draw to it every frame.
* **More variables** - I have to save a lot of objects with PIXI so that I can modify them later. This makes the code more verbose and sometimes less readable.

### Conclusion

It's been a long refactoring journey, but the end is finally in sight. Next up, I'm going to start working on better visual cues and effects, with the benefits of a much cleaner code-base and a more powerful rendering engine.

I hope this provided you with some useful information, and I'll be back with some juicier updates soon!
