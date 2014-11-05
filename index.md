---
layout: home
---

Don't know what MoonSharp is ? <a href="about.html" class="alert-link">Read here.</a>.

Latest release: <a href="changelog.html" class="alert-link">0.6.0</a>.

Next scheduled release: <a href="https://github.com/xanathar/moonsharp/milestones"  class="alert-link">14 Nov 2014.</a>

#### 2014-11-05

MoonSharp has a new logo, thanks to Isaac, a friend in the Grimrock modding community!

On more techincal things.. as a peek of new functionality: a dynamic expression facility has been added.
In this way, .NET code may evaluate expressions without compiling Lua bytecode using the current
execution context, and the same can be done by Lua code.

The evaluation will always perform raw access and will never call any function - it can be used to implement
debuggers or things like a data load from a table source (using tables as if it was a format like JSON.. except Lua).

The evaluation is totally dynamic. So for example, if the expression contains a reference to a variable "x", and we are inside a whatsoever function, it might evaluate as a global. Reevaluate the same expression when a function with an upvalue (or local, or argument) "x" is on the stack, and "x" will refer to that upvalue/local/argument!


#### 2014-11-04

Version 0.7.0 will be probably out in a week, but don't hold your breath as it will be the biggest MoonSharp release so far, so the probability of it slipping is high.

Hopefully I will be able to release MoonSharp 0.7.0 before the week of Nov 17th, otherwise it will slip for another week or two.

That week I'll be attending O'Reilly Velocity Europe 2014 in Barcelona.. if anyone is there and wants to talk of MoonSharp things, let me know, at info@moonsharp.org.


#### 2014-10-21

Minor changes have been done to how escape sequences in strings are managed.

Also, the sharing objects and error handling tutorials have been added!



#### 2014-10-20

Version 0.6.0 has been released. This is a very big release and contains - above all - a complete string library and a vastly
improved error reporting for both syntax and runtime errors. <a href="changelog.html" class="alert-link">See the changelog</a> for 
a complete list of changes.

Note: there are a couple of compatibility breaks in this version - nothing that can't be fixed in 5 minutes, but you've been 
warned.


#### 2014-10-17

The string library has been completed. 
For the sake of not reinventing the wheel (or, at least, not reinventing *every* wheel), it was ported from KopiLua code.
In the process, a compatibility layer has been implemented to ease the porting of code using classic stack-based APIs like
KopiLua, UniLua or Lua itself.

On a side note, visit <a href="http://www.darkdale.net" class="alert-link">www.darkdale.net</a>. It's the project of a friend, 
a dungeon crawler on the spirit of Eye of the Beholder, Lands of Lore and Legend of Grimrock and it uses MoonSharp as a 
scripting engine!



#### 2014-10-08

MoonSharp 0.5.5 has been released. 

* Optimized parsing stage
* Performance statistics for diagnostics



