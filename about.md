---
layout: home
---

MoonSharp is a Lua interpreter written entirely in C# for maximum compatibility in .NET, Mono and Unity.

It's a "clean room" design - that is, almost none of the original source code has been reused (apart from parts of the standard
library) and most of it has been developed by studying the specifications and the behavior of the official implementations.



### Features
* Easy to use
* Written completely in C# + ANTLR, for the biggest portability in CLR implementations (.NET, Mono, Unity)
* Covers all Lua 5.2 constructs except goto/labels (for now)
* Works on .NET 3.5 and Mono 2.6 and later. In roadmap is a .NET 4.x portable class library version for Windows Phone and Silverlight platforms.


### Project Status

 
* Some language differences exist - [see here](moonluadifferences.html)
* All Lua 5.2 language constructs are implemented, except goto/labels. 
* Development of the standard library is almost finished. Updated situation in [this pdf](http://www.moonsharp.org/MoonSharpStdLib.pdf)
* MoonSharp/.NET integration is 99% completed (and works great).
* The debugger "almost" works on the bytecode level. Source level is not yet supported. Currently as a Windows.Forms application, a better solution might be planned sooner or later - TBC
* Coroutines and state-save support has not been started yet.
 

#### Roadmap

* support for all core language structures (see documentation for differences between MoonSharp and Lua)
* compatibility with all applyable tests in [Lua Test Suite](http://www.lua.org/tests/5.2/) and [Lua Test More](http://fperrad.github.io/lua-TestMore/). Most tests of the suite are passing.
* debugger embeddable in applications using MoonSharp
* REPL interpreter
* standard library  
 

 
### License

The program and libraries are released under a 3-clause BSD license - see the license section.

This work is based on the ANTLR4 Lua grammar Copyright (c) 2013, Kazunori Sakamoto.

Some parts of the string library (parttern matching and string.format) are taken from [KopiLua](https://github.com/NLua/KopiLua)
