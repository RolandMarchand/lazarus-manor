---
author: Lazarus Overlook
title: How to Play Doom
date: 2021-09-26
categories:
    - Video Games
tags:
    - Doom
    - Video Games
    - Modding
---
## Why Can't We Just Play Doom
Playing Doom should be straight forward, you just get the game and you play it, right ? Well, it's a bit more complicated than that…

Doom is old, like very old. It was made in the 90's for DOS, so even if you can somehow find DOOM.EXE, it won't even execute on a modern Windows 10 machine. Even if you could execute it with DosBOX, you probably wouldn't want to. Except if you're looking for amazing features, such as:

* Locked 4:3 320x200 resolution
* Hard coded 35 FPS
* Engine bugs
* Funky mouse support
* Terrible music player
* Limited modding

So, are we doomed ? Don't worry, as we have the power of [Open Source](https://github.com/id-Software/DOOM)!

In 1999, the source code of Doom for Linux was released by ID software (the DOS build was never released because of [licensing issues](https://doomwiki.org/wiki/Doom_source_code)). So the Doom community got hard at work for decades to create great ways to play this wonderful game. With their efforts, they managed to uncap hard coded limits, designed lots of high quality maps, scripting languages and mapping tools, establish a vibrant multiplayer scene, integrate OpenGL and Vulkan, make the game [run inside of a camera](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fi.kym-cdn.com%2Fentries%2Ficons%2Ffacebook%2F000%2F021%2F839%2F1462755712852568.jpg&f=1&nofb=1), fix a large amount of bugs and much, much more.

All of that for free and on every platform! You only need a WAD file and a source port.

## WADs
### IWADs
Those are the main files, which you only need one of. It contains all the important data like maps, sprites, sounds and music, among others.

[There are actually numerous IWADs](https://doomwiki.org/wiki/IWAD), but I recommend getting DOOM2.WAD, since it is a great game by itself and is the most supported in terms of modding.

You can easily find IWADS online, but that would be consider piracy. The developers have [publicly stated](https://twitter.com/ID_AA_Carmack/status/456974084857802752?s=20) that they don't care much for that, but if piracy isn't your cup of tea, you can legally buy the game off [Steam](https://store.steampowered.com/app/2300/DOOM_II/) or [GOG](https://www.gog.com/game/doom_ii_final_doom).

I personally recommend the latter, as it is [DRM-Free](https://www.gog.com/forum/general/what_does_drmfree_mean).

But if you're not willing to pay, nor to pirate, there is the community-made IWAD called [Freedoom](https://freedoom.github.io/).

### PWADs

Those are contains modifications for Doom, might they be new maps, enemies, musics or others. Some could be classified as extensions, others as quality of life improvements, or even as brand new games altogether.

You can load as many PWADs you want, considering compatibility.

What you must keep in mind is the type of mod, so for what engine it was made for. These types are the most important:

* **Vanilla**

	These ones are made for the original doom and have the same limitations as the original engine. They can be played by any doom engine, old or new. They do not have new or advanced features.

* **Limit-removing**

	Very similar to vanilla, except when it comes to [engine limits](https://doomwiki.org/wiki/Static_limits), such as being limited to 6 MB of ram, 128 rendered sprites at a time or 16 active switches at a time.

* **Boom**

	Boom was a community-made engine built to remove limits and add a bunch of new features like transparent walls, conveyor belts or friction on certain surfaces like ice or mud.

	These mods still feel very much like Doom, but they must be played on a Boom-Compatible engine.

* **ZDoom**

	These mods are made for the ZDoom engine, which provides an [insane amount of features](https://doomwiki.org/wiki/ZDoom#Features). So these mods tend to be the most impressive, like [Brutal Doom](https://www.moddb.com/mods/brutal-doom), but can also seem out of place with how different they are from the original Doom.

	Only ZDoom-based engines can play those mods.

	The files often end in .PK3.

* **Dehacked**

	These mods are special, they're not made for a specific engine nor are they really used to add content. They are made by a special editor to change how the game fundamentally works. They can change hit points, sounds, animation sequences and others.

	Almost all engines support them in one way or another.

	The files end in .DEH.

There are also other types of mods like [Marine's Best Friend](https://doomwiki.org/wiki/MBF), but those are rare and not as important right now.

Finding WADs can be tricky, but [this article](https://doomwiki.org/wiki/Best_Doom_mods) gets you covered.

## Ports
### [Chocolate Doom](https://www.chocolate-doom.org/wiki/index.php/Chocolate_Doom)
This is the true Doom experience, as vanilla as it can get, with all the limitations mentionned before. [Its philosophy](https://github.com/chocolate-doom/chocolate-doom/blob/master/PHILOSOPHY.md) is to be as similar as possible to the original. It even has the same bugs.

It is limited as far as modding goes, obviously only accepting vanilla-compatible mods.

Even if you want a vanilla experience, I would only recommend to play this port for historical purposes, or to read its source code to learn how to program.

If you want a pleasurable, original experience, I recommend its friendly fork, Crispy Doom.

### [Crispy Doom](https://www.chocolate-doom.org/wiki/index.php/Crispy_Doom)
Chocolate Doom, but *better*.

Crispy Doom is my port of choice, it's simple, it works, and it's light.

Crispy Doom is based on Chocolate Doom and improves upon it by adding quality of life features such as uncapped frame rate, widescreen support, removed limits and many more. But it does not add so many features as to make the game unrecognizable.

It still is limited in terms of modding, as it can only play vanilla and limit-removing WADs.



### [PRBoom+](http://prboom-plus.sourceforge.net/)
This is an *advanced* port with true high resolution, insane opitmization and an uncapped framerate. It has OpenGL support and is the ultimate demo recording port. It has a myriad of features and is optimized for speed.

Even with all of those goodies, it still provides an authentic Doom experience, and can play almost every single mod out there, aside from ZDoom mods discussed further down.

The list of issues is short, but it encompasses a gamma that's way too high by default, and the requirement to set its port compatibility manually through [complevels](https://www.doomworld.com/forum/topic/54491-what-is-complevel/).

### [GZDoom](https://www.zdoom.org/downloads)
This is the most popular, powerful and featureful port out there. Based on ZDoom and often considered as the go-to source port, it is capable of playing almost every single mod.

GZDoom is packed with features such as:

* Decorate and Zscript.
  They enable GZDoom to transcend Doom, like [Comatose](https://doomwiki.org/wiki/Comatose).

* OpenGL, lighting, true color, slopes, portals, high res and model support.

* An incredible amount of configurable options.

* Can play every single Doom Engine games.

As amazing as GZDoom is, there are reasons why you shouldn't consider it as the end all be all for source ports:

* It's badly optimized.
  At least compared to the other source ports mentioned, it doesn't run  well on low-end machines, even with its lite version, [LZDoom](https://www.zdoom.org/wiki/LZDoom).

* It has absolutely terrible default settings, and there are a lot of them.

* It has little to no demo support.

* There are some gameplay quirks that differ from the original Doom. e.g; Lost Souls count as enemies

Though, with all of that said, there is a reason why this source port is loved by many, and I encourage you to try it out

### [Zandronum](https://zandronum.com/)
A multiplayer port!

Zandronum is based on ZDoom (previous version of GZDoom), but focuses on multiplayer and is the go-to for that purpose.

All the ports mentioned before can do multiplayer, but it is not recommended. Zandronum's the man.

It can be used as a single-player source port, but that's equivalent to using a 3 years old version of GZDoom.
