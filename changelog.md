---
layout: default
title: Change Log
subtitle: 
---

#### Coming soon 
**Available on master branch** <span class="label label-warning">Unreleased</span>

Nothing yet.

#### Version 1.1.1.0
** Released on 2015-12-08

* New feature: <a href="coroutines.html#preemptive">Preemptive coroutines</a> to limit time spent in scripts while preserving state.
* Added Script parameter to custom converters - this might break compatibility if you were getting custom converters. (Issue #118)
* Added methods to easily build an array table from a DynValue array.
* Fixed issue #117 - long empty comments not lexed correctly.


#### Version 1.0.0.0
** Released on 2015-10-22

* Added a PropertyTableAssigner facility which allows POCO objects made of properties to be filled by deserialization of a table.
* Added a DebuggerEnabled property so that the debugger can be disabled for specific scripts invocations (issue #113)
* Script loading now uses an access mode which allows shared operations (thanks Atom0s)
* Fixed: Capturing varargs in table from inner scope causes null ref exception (issue #110)
* Fixed: Event re-registration might now work (issue #112)
* Fixed: error messages in load and loadfile functions were at times unhelpful (issue #107)


#### Version 0.9.6.2
**Released on 2015-06-15** <span class="label label-success">New</span>

* Fixed a major bug in how variable arguments were handled (issue #92)
* Fixed a problem with the registration of extension types (issue #100)
* Fixed a potential incompatibility with IronPython and assembly auto-discovery (issue #96)
* Fixed an invalid datetime format specifier in os module (thanks edwingeng) (issue #99)
* Ownerships checks added to scripts, tables and closures (issue #93)


#### Version 0.9.6.2
**Released on 2015-06-15** 

* Fixed a bunch of bugs concerning tuples and variable arguments (``...``) functions (See issue #92)
* Added checks for illegal cross-script usage of some types (See issue #93)


#### Version 0.9.6
**Released on 2015-05-22** 

* Support for registration of generic type definitions (e.g. ``List<>``)
* Support for registration of generic extension methods  
* Support for registered multidimensional arrays
* Support for user-defined member descriptors
* Support for calling members with ``__call`` metamethod through a ``Script.Call`` method
* Revised support for registered array types (was breaking on some Mono versions)
* Fixed: indexers not working on multiple base type dispatch



#### Version 0.9.4
**Released on 2015-05-20** <span class="label label-success">New</span>

* Interop with enums !
* Async methods on .NET 4.x (normal and PCL) supporting async/await
* Revised standard descriptors architecture for better extensibility
* Improved support for nested types
* Improved error messages when an invalid member access is made
* Interop support improvements for value types passed as userdata
* Interop support for events (with limitations)
* Interop support for varargs (ParamArray) functions
* Improved interop with const and readonly fields
* Improved interop with mixed access properties
* DynValue.ToDynamic method for easier use on .NET 4.x (normal and PCL)
* Extended support for iterating over coroutines with a for..each loop
* Adjusted constructors visibily for StandardUserDataPropertyDescriptor and StandardUserDataFieldDescriptor
* Partial support to directly use a Lua coroutine as a Unity3D coroutine 
* ScriptLoaderBase and all its child can optionally ignore the LUA_PATH global
* The REPL interpreter and ReplInterpreterScriptLoader now consider the LUA_PATH_5_2 environment variable
* Fixed a bug on Unity3D where UnityAssetsScriptLoader was not available on WSA apps
* Fixed a bug where using AutoRegistration break automatic delegates interop
* Fixed some issues on explicitely registered collections (See github issue #88)
* Fixed documentation issues
* Workaround a Unity3D bug where optimized access on a const field crashed the editor




#### Version 0.9.2
**Released on 2015-03-31** 

* Support for lambda style anonymous function as done by metalua (``|x,y| x*y`` for ``function(x,y) return x*y end`` )
* Support for interop with overloaded methods (see note 1 below)
* Support for interop with fields (see note 2 below)
* Support for interop with methods having ref/out parameters (see note 2 below)
* Support for interop with C# indexers (incl. language extension)
* Support for interop with overloaded operators and metamethods on userdata
* Support for interop with extension methods
* Partial support for iterating over coroutines with a for..each loop
* Proper REPL interpreter and facilities implemented
* FIXED: % operator was calling the __div metamethod instead of __mod

Note 1: overloaded methods add computational complexity at every call to dispatch to the correct method.
While caching exists to improve performance, please try to avoid that.

Note 2: setting a field and methods with ref/out paramenters at the moment always use reflection, whatever
the InteropAccessMode is. Use properties and/or methods returning DynValue tuples for faster access.





#### Version 0.9.0
**Released on 2015-03-23** 

* Removed ANTLR dependencies
* Improved performances at load methods
* string.dump implemented
* Bytecode serialization supported
* Goto statements supported
* Hex floats support
* Tail call optimization support
* Invalid comparison functions in table.sort now correctly report errors
* Better coverage of error messages at the lexer level
* I/O streams are now customizable.
* Support for conversions to List<T>, IList<T>, T[], Dictionary<K,V> and other generic collection types
* Support for customizable type converters
* Constructors can be called on userdata (by calling a fictitious __new method on a static userdata)
* tonumber() supports all bases between 2 and 10 (thanks jerneik)
* Fixed a bug where the debugger connected to the wrong hostname [Flash security is perverted]
* Fixed a potential memory leak and other hypotetical issues when a deep nested *break* is executed
* Compatibility with mono --full-aot (which should include iOS support on Unity / Xamarin)
* Test suite fixed for an issue where the wrong optimization mode on userdata is chosen
* A lot of reordered things in code.. a lot of easily repairable breaking changes, sorry
* Extended XML help coverage 
* Version for portable.NET 4.x and .NET 4.x frameworks
* Tested on Windows Store Apps (win8/8.1)
* Tested on Windows Phone 8.1
* Tested on Silverlight 5
* Tested on iOS (minor issues still to be solved)





#### Version 0.8.5.1
**Released on 2015-01-13**


* Vastly improved type descriptors  - #39
* Fixed: Varargs not supported on main chunk  - #46
* Fixed: pcall returns only the first return value  - #47
* Fixed: Threading check not working properly  - #42
* Fixed: Exception ctor overloads revised for potential obscure bugs  - #40
* Fixed: String patterns do not support \0 characters - %z must be used instead.  - #29


<a href="https://github.com/xanathar/moonsharp/releases/tag/v0.8.5.1">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 




#### Version 0.8.2.1
**Released on 2014-12-24**

This is a bug fix release over 0.8.1:

* Function in lua which has 2 parameters with the same name throws error
* UserData with private properties throws on UserData.RegisterType
* table.unpack raises IndexOutOfRangeException
* Assigning nil to a table field should delete it
* Minor/partial fix on the multithread check on script execution

**Note: 0.8.2.1 has not been released on NuGet at the moment**

<a href="https://github.com/xanathar/moonsharp/releases/tag/v0.8.2.1">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 




#### Version 0.8.1
**Released on 2014-12-18** 

This is a bug fix release over 0.8.0:

* A **lot** of bug-fixes regarding error handling on nested C# calls and coroutines
* A bad bug involving a miscalculation of table length has been fixed (See issue #38)

<a href="https://github.com/xanathar/moonsharp/releases/tag/v0.8.1">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 




#### Version 0.8.0
**Released on 2014-12-11**

* 'os', 'io' and 'file' libraries completed. Note that in Unity some functionality is not supported.
* Parts of the 'debug' library completed. This has been done for compatibility sake, as debuggers are implemented differently in MoonSharp.
* Improved error messages
* Improved options management [breaking change!]
* Table indexers now support any number of keys and resolve to subtables automagically
* Fixed a bug where a piece code behaved differently in .NET 4 than in .NET 2 (weird covariance stuff, this btw is interesting stuff).
* Support for Unity Web Player and Unity Android (non-stripped)
* Test suite runs on Unity Windows, Unity Android and Unity Web Player correctly!
* Remote Debugger: it's now possible to attach the debugger to running scripts without having them getting paused
* Remote Debugger: it's now possible to break on errors, and to select which errors break and which don't
* Remote Debugger: added set breakpoint and clear breakpoint commands




#### Version 0.7.0
**Released on 2014-11-07** 

* Remote debugger support! <a href="demo/MoonSharpDebuggerDemo.zip">Little demo here.</a>
* \u{xxx} escape to output unicode codepoints, as defined in Lua 5.3
* Better handling of some lexer errors with invalid escape sequences
* Solved a bug where the VM debugger crashed on inspecting globals
* ScriptFunctionDelegate types, and added GetDelegate methods in Closure type
* Changed scope implementation of functions, so that _ENV is always present, either as a local or upvalue (previously, if a function never used globals, it didn't reference _ENV). This fixes sandboxing of functions calling load or equivalents in some remote scenarios.
* Support for dynamic expressions in .NET
* Support for dynamic expressions in standard library (addition)
* Fixed some license issues here and there




#### Version 0.6.0
**Released on 2014-10-20** 

* A new DataType of Void introduced to correctly handle the no return value scenario.
* Fixed a bug where multiple return values could not be used directly in an operation
* New modes for easier type conversions in CallbackArguments. Plus, DynValue.CheckType method.
* Changed "Closure" property of CallbackFunction from Table to be named AdditionalData and being an Object
* Removed some deprecated methods from ScriptExecutionContext
* Lots of optimizations
* Fixed a bug where strings containing some escape sequences were not parsed correctly.
* Improved syntax error messages
* Made ':' calls work the same as '.' calls on userdata
* String library completed, with some methods importing code from KopiLua, ported to 5.2 standard
* Fixed some bugs in the parsing of string literals escape codes
* Started a small wrapper to interface with code using the classic stack-based API (UniLua, KopiLua or Lua itself).



#### Version 0.5.5 
**Released on 2014-10-08**

* Optimized parsing stage
* Performance statistics for diagnostics



#### Version 0.5.3 
**Released on 2014-10-02**

* Fixed a number of bugs in string library
* Fixed a nasty bug where the code accessed a "C:\temp" directory, likely failing
* Added a bunch of helpful overloads and helper methods



#### Version 0.5.0 
**Released on 2014-09-30**

* Support for metatables
* pcall
* Standard library
* Coroutines
* Marshalling of C# objects as userdata

Previous versions are not detailed.


