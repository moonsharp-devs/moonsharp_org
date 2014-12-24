---
layout: default
title: Change Log
subtitle: 
---

#### Coming soon 
**Available on master branch** <span class="label label-warning">Unreleased</span>

Nothing.. yet.


<hr />

#### Version 0.8.2.1
**Released on 2014-12-24** <span class="label label-success">New</span>

This is a bug fix release over 0.8.2.1:

* Function in lua which has 2 parameters with the same name throws error
* UserData with private properties throws on UserData.RegisterType
* table.unpack raises IndexOutOfRangeException
* Assigning nil to a table field should delete it
* Minor/partial fix on the multithread check on script execution

**Note: 0.8.2.1 has not been released on NuGet at the moment**

<a href="https://github.com/xanathar/moonsharp/releases/tag/v0.8.2.1">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 


<hr />

#### Version 0.8.1
**Released on 2014-12-18** 

This is a bug fix release over 0.8.0:

* A **lot** of bug-fixes regarding error handling on nested C# calls and coroutines
* A bad bug involving a miscalculation of table length has been fixed (See issue #38)

<a href="https://github.com/xanathar/moonsharp/releases/tag/v0.8.1">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 


<hr />

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


<hr />

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


<hr />

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

<hr />

#### Version 0.5.5 
**Released on 2014-10-08**

* Optimized parsing stage
* Performance statistics for diagnostics

<hr />

#### Version 0.5.3 
**Released on 2014-10-02**

* Fixed a number of bugs in string library
* Fixed a nasty bug where the code accessed a "C:\temp" directory, likely failing
* Added a bunch of helpful overloads and helper methods

<hr />

#### Version 0.5.0 
**Released on 2014-09-30**

* Support for metatables
* pcall
* Standard library
* Coroutines
* Marshalling of C# objects as userdata

Previous versions are not detailed.


