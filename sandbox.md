---
layout: tutorial
title: Sandboxing
subtitle: Restricting what Lua script can do
---


> This section is under construction


#### Two words about sandboxing

More likely than not, sandboxing is a key feature in your MoonSharp usage. Unless you control, in some way, the script providers (and maybe also the users), there is a fundamental trust problem in loading scripts (or any kind of code and sometimes even data) at runtime: security.

For example, let's say that you are writing a videogame and you use Lua to make the game "moddable". Users can go to some community website and download a mod containing 
new levels, AIs and what-not all scripted using MoonSharp for the maximum flexibility. Everybody is happy. Now think what would happen to your game's reputation if some
evil user uploads some malicious scripts well hidden in some beautiful level.. like a script creating an .exe file on the desktop, where other users are likely to click 
accidentally. 

You don't want to expose some APIs (like 'os', 'io' and 'file') to the general users unless either you trust them or it's plainly evident that malicious use is possible.


#### Sandboxing in Lua itself






To be put here:

* Sandboxing techniques in Lua itself
* Selecting loaded modules
* Overriding the script loader


