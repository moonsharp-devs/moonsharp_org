---
layout: default
title: Differences between Moon# and Lua
subtitle: The dark side of the moon
---

Here is a list of differences between Moon# and Lua. This is of course subject to heavy changes as the development on Moon# goes on.

#### List of differences

* Strings are Unicode: some caution is required if code uses strings to store binary data. On the other hand, strings used as real strings are a lot easier to use.
* Weak tables are not supported
* The standard library may at points be wildly different - Check [here](https://docs.google.com/spreadsheets/d/1Iw8YMSY8N0tGEyaD-vmmJnlaQ5te4P4CqTXYpEiSEL8/edit#gid=0) for an overview of the standard library status.
* Garbage Collection is different, as Moon# relies on .NET/Mono standard GC
* Compatibility only at the source level - no luac, no saved state compatibility, no lua binaries
* Error messages are at times different 
* Hexadecimal floats are not supported

#### List of additions

* A non-yieldable __iterator metamethod has been added. It's called if the argument *f* of a "for-each" loop is not actually a function.
* A default iterator looping over values (not keys) is provided if the argument f of a "for-each" loop is a table without __iterator or __call metamethods
* \u and \U escapes, followed by 4 and 8 hex digits respectively, are supported inside strings and will output the specified Unicode codepoint


#### Rationale

There are several reasons for these differences, but the most important ones are:

* Moon# will be modeled on the .NET architecture viewed through C# colored lenses, rather than a posix architecture viewed through C lenses
* The architecture is different and some things simply cannot be done with a reasonable effort (or my low experience! ;) )
* The purpose is offering scripting facilities to C# apps at the fastest speed possible. The entire purpose of Moon# is to sacrifice Lua raw execution speed in order to gain performance on Lua/C# cross calls.




        
		
		
		


