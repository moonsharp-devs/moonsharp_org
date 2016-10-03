---
layout: default
title: Differences between MoonSharp and Lua
subtitle: The dark side of the moon
---

Here is a list of differences between MoonSharp and Lua. 
This is of course subject to heavy changes as the development on MoonSharp goes on.


#### List of differences

* Strings are Unicode: some caution is required if code uses strings to store binary data. On the other hand, strings used as real strings are a lot easier to use.
* Weak tables are not supported
* Very very minor differences in some parts of the standard library - Updated situation in [this pdf](http://www.moonsharp.org/MoonSharpStdLib.pdf).
* Garbage Collection is different, as MoonSharp relies on .NET/Mono standard GC
* Compatibility only at the source level - no luac, no lua binaries
* Error messages are at times different (though most of the times they are the same)
* Function names are tracked at function declaration, not backtracked at function calls like in Lua (noticeable only when debugging)
* Lua does not document (AFAIK) which functions can yield and which not. For example ``tostring()`` can yield in MoonSharp but not in Lua. See [this pdf](http://www.moonsharp.org/MoonSharpStdLib.pdf) for a documented list of what can yield in MoonSharp. Generally you can expect to be able to yield in more places and not less, but your mileage may vary.


#### List of additions

* Multiple expressions can be used as indices but the value to be indexed must be a userdata, or a table resolving to userdata through the metatable (but without using metamethods). So, for example, ``x[1,2,i]`` is parsed correctly but might raise an error if ``x`` is not a userdata.
* Metalua short anonymous functions (lambda-style) are supported. So ``|x, y| x + y`` is shorthand for ``function(x,y) return x+y end``. 
* A non-yieldable ``__iterator`` metamethod has been added. It's called if the argument ``f`` of a ``for ... in ... `` loop is not actually a function.
* A default iterator looping over values (not keys) is provided if the argument ``f`` of a ``for ... in ..." loop is a table without ``__iterator`` or ``__call`` metamethods
* ``\u{xxx}`` escapes, where x are up to 8 hexadecimal digits, are supported inside strings and will output the specified Unicode codepoint, as it does in Lua 5.3
* ``loadsafe`` and ``loadfilesafe`` methods, same as ``load`` and ``loadfile`` but defaulting to the current top-of-the-stack ``_ENV`` instead of the default one, for easier sandboxing
* The ``dynamic.eval`` and ``dynamic.prepare`` functions to handle dynamic expressions
* ``string.unicode`` method, just like ``string.byte`` but returning a whole unicode codepoint
* ``string.contains``, ``string.startsWith`` and ``string.endsWith`` methods allow easier and faster performing string ops without having to rely on patterns for simple scenarios


#### Rationale

There are several reasons for these differences, but the most important ones are:

* MoonSharp will be modeled on the .NET architecture viewed through C# colored lenses, rather than a posix architecture viewed through C lenses. For example it doesn't make sense to treat strings like Lua does (plain old char ptrs, sometimes and sometimes not interpreted as utf-8).
* The architecture is different and some things simply cannot be done within a reasonable effort 
* The purpose is offering scripting facilities to C# apps at the fastest speed possible. The entire purpose of MoonSharp is to sacrifice Lua raw execution speed in order to gain performance on Lua/C# cross calls.
* Lua serves the dual purpose of being a stand-alone language AND an embeddable scripting language. MoonSharp takes choices which favors the embeddable scenario at the expense of the stand-alone one.
* Worth repeating, MoonSharp does not aim at 100% compatibility, it aims at giving Lua scriptability to C# applications in the best possible way.





        
		
		
		


