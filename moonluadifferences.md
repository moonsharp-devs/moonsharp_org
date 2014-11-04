---
layout: default
title: Differences between MoonSharp and Lua
subtitle: The dark side of the moon
---

Here is a list of differences between MoonSharp and Lua. This is of course subject to heavy changes as the development on MoonSharp goes on.

#### List of differences

* Strings are Unicode: some caution is required if code uses strings to store binary data. On the other hand, strings used as real strings are a lot easier to use.
* Weak tables are not supported
* The standard library may at points be slightly different and parts not supported - Updated situation in [this pdf](http://www.moonsharp.org/MoonSharpStdLib.pdf).
* Garbage Collection is different, as MoonSharp relies on .NET/Mono standard GC
* Compatibility only at the source level - no luac, no lua binaries
* Error messages are at times different (though most of the times they are the same)
* Hexadecimal floats are not supported


#### List of additions

* A non-yieldable __iterator metamethod has been added. It's called if the argument *f* of a "for-each" loop is not actually a function.
* A default iterator looping over values (not keys) is provided if the argument f of a "for-each" loop is a table without __iterator or __call metamethods
* \u{xxx} escapes, where x are up to 8 hexadecimal digits, are supported inside strings and will output the specified Unicode codepoint, as it does in Lua 5.3



#### Rationale

There are several reasons for these differences, but the most important ones are:

* MoonSharp will be modeled on the .NET architecture viewed through C# colored lenses, rather than a posix architecture viewed through C lenses
* The architecture is different and some things simply cannot be done with a reasonable effort (or my low experience! ;) )
* The purpose is offering scripting facilities to C# apps at the fastest speed possible. The entire purpose of MoonSharp is to sacrifice Lua raw execution speed in order to gain performance on Lua/C# cross calls.




        
		
		
		


