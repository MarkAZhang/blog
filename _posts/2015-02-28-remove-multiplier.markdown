---
layout: post
title: "How a Score Multiplier can Ruin Your Game"
date: 2015-02-28 00:17:32
author: Mark Zhang
---


Lately, I've noticed a worrying trend: new players of [Impulse](http://play-impulse.com) have been throwing their hands up in frustration within the first few levels. Turns out the score multiplier was to blame.

I loved DDR as a kid. In particular, I loved getting full-comboes. It's a sweet, sweet feeling to face down a 300BPM song, get pushed to your physical limit...and not make a single mistake. When I added Impulse's score multiplier, I wanted it to feel like that.

<div class="post-image-container">
  <img src="{{ site.baseurl }}/assets/img/ddr_full_combo.png"/>

  <div class="image-caption"><i>Awwwwwww yeahhhhhh</i></div>
</div>

In Impulse, your multiplier increases whenever you kill an enemy, and resets whenever an enemy hits you. The goal is to reach a score threshold before the level gets so hard that it overwhelms you.

The only way to die in Impulse is to get knocked into the Void. By penalizing the player for sloppy play, the score multiplier  makes the game that much more intense and adds another dimension of skill. Every moment, every little action, matters. Repeatedly losing your multiplier is a slow but sure death.

I liked where this was going, so I went ahead and added the score multiplier. It felt AWESOME. Whenever I beat a hard level untouched, I felt like a badass.

<div class="post-image-container">
  <img src="{{ site.baseurl }}/assets/img/multiplier.png"/>
  <div class="image-caption"><i>Keeping a high multiplier is key to beating harder levels.</i></div>
</div>

But all was not well. New players were quickly giving up on my game. It took me a while to realize that the score multiplier was responsible.

The gist was this: new players didn't care about the multiplier. They were still learning the ropes, and didn't have the mechanics down to get a high multiplier anyways. The multiplier was complicating the initial experience without any real benefit.

There was an even deeper problem.

I didn't want levels to be beatable in 10 seconds, and since the multiplier could quickly send scores soaring, there was a high lower bound on the score thresholds. However, new players couldn't keep any reasonable multiplier, so levels ended up being unreasonably long. After playing minute-long levels and dying repeatedly, players just gave up.

<div class="post-image-container">
  <img src="{{ site.baseurl }}/assets/img/early_death.png"/>
  <div class="image-caption"><i> Impulse expects you to die a lot. Minute-long levels are unbearable.</i></div>
</div>

I ended up removing the multiplier for easy-mode, and introducing it as a new mechanic in hard-mode. This had several benefits:

* There is less to learn at the beginning of the game.
* The early levels feel shorter and their length can be carefully controlled.
* There is now an additional mechanic to distinguish hard-mode from easy-mode. Hard-mode uses the same level layouts as easy-mode, and I want the modes to feel distinct.

Removing the multiplier has made a huge difference. Players are getting much further into the game, and they're much more excited and engaged.

Lesson learned: A score multiplier can be a great way to add intensity and an extra dimension of skill to your game, but it can also damage the experience for newer players. 

Hope this was helpful for other game developers who are trying to make challenging [but not punishing](https://www.youtube.com/watch?v=ea6UuRTjkKs) games.

