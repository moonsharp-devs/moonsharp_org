---
layout: default
title: FAQ
subtitle: Freaky answers questioned.
---

##### What is the development status ?

The interpreter runs correctly and fast enough for the common needs. It strikes a unique balance in performance and ease of use, specially when heavy interoperation with CLR code is needed.

The standard library is missing some methods, but is converging fast enough. Still missing are io, os and file. Everything else is either there (most 
things) or won't be (debug). See <a href="http://www.moonsharp.org/MoonSharpStdLib.pdf">this pdf for more info</a>.

A version targeting .NET 4.x portable class libraries needs to be done.

Previously on this list were coroutines, string library, debugger and other things which weren't completed. They are, now ;).


##### Are Lua and MoonSharp 100% compatible ?

Of course not. There are many reasons for this (including bugs), but you can read more about [the differences here](moonluadifferences.html).
The differences are unlikely to hit you on daily usage, as they are all encountered in not so common scenarios.

If you find any program which runs differently on the two system and it's not cited in the differences page, please report on the forums!


##### Why Lua ?

It's a cool language. MoonSharp flirts a lot with videogame development in Unity, and Lua has been the scripting language of choice for
videogames for years. Also I thought I knew Lua a lot when I started this project, so it seemed a natural choice. The first thing I learned was that,
no, I didn't know the language well enough. It's been an amazing experience.


##### Is MoonSharp usable only in videogames ?

No technology ever can be used only in one programming scenarios, though some are better fits than others. 

A business application running Lua is surely more common than videogames running XSLTs or Razor, anyway. 

I, for one, am using MoonSharp in business-line applications, and so can you, if you want.


##### How can I receive support ?

Write on the forums. Hopefully somebody - maybe me - will answer. 


##### Can I help edit this website ? How was the website built ?

Sure! Just fork [https://github.com/xanathar/moonsharp_org](https://github.com/xanathar/moonsharp_org) on GitHub.

It's a website built using [Jekyll](http://jekyllrb.com/) and [GitHub Pages](https://pages.github.com/). 

Send me a merge request after the edits have been done.


##### Can I help the project ?

Again, sure! Just fork [https://github.com/xanathar/moonsharp](https://github.com/xanathar/moonsharp) on GitHub.

Send me a merge request after the edits have been done.



        
		
		
		


