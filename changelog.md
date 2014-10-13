---
layout: default
title: Change Log
subtitle: (since 0.5.0)
toadd: Emulation of classic stack based callback mode (LuaState mode), to help integrating callbacks done in C for Lua and in C# for UniLua and KopiLua
---

#### Coming soon 
**Available on master branch** <span class="label label-warning">Unreleased</span>

* A new DataType of Void introduced to correctly handle the no return value scenario.
* New modes for easier type conversions in CallbackArguments. Plus, DynValue.CheckType method.
* Changed "Closure" property of CallbackFunction from Table to be named AdditionalData and being an Object
* Lots of optimizations
* Fixed a bug where strings containing some escape sequences were not parsed correctly.
* Improved syntax error messages
* Made ':' calls work the same as '.' calls on userdata
* Partial string.format implementation.


#### Version 0.5.5 
**Released on 2014-10-08** <span class="label label-success">New</span>

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



