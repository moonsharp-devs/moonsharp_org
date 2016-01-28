---
layout: home
---

Don't know what MoonSharp is ? <a href="about.html" class="alert-link">Read here.</a>.

Latest release: <a href="changelog.html" class="alert-link">1.2.1.0</a> \| <a href="https://github.com/xanathar/moonsharp/releases/download/v1.2.1.0/moonsharp_release_1.2.1.0.zip">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 


#### 2016-01-28

Just as a little peek into what is coming. Soon. 

Work is going on towards reaching IL2CPP maximum compatibility. Let's clarify. As now MoonSharp has quite a good degree of compatibility with IL2CPP but it requires registering the scripts manually (or using DoString instead of DoFile) and a lot of painful fiddling with the ``link.xml`` file. 

But why fiddling with reflection when one can use no reflection at all ? Soon MoonSharp will include a little utility which will take a snapshot of what types are registered for script interop, and will create a C# source file on the fly which allows for no reflection (thus maximum IL2CPP compatibility), faster startup and faster execution, specially on AOT/IL2CPP platforms (like iOS) and in corner cases. At the price of a little more workflow effort (but very little) and some little extra requirements on interop types.. which can handily be solved through proxy objects.

If you are not using Unity or Xamarin, you probably have less reasons to rely on these (desktop .NET and Mono are a lot more powerful), but you'll still benefit from faster startup and execution.

Stay tuned for more anticipations.


#### 2016-01-24

MoonSharp 1.2.1.0 has been released!

* Support for <a href="proxy.html">proxy objects</a> - a nice way to encapsulate objects exposed to script and create the base for many clever tricks
* Support for sub-objects in property assigners
* MoonSharpHide attribute to specify members not to be exposed to Lua scripts
* MoonSharpVisible attribute now can be added to constructors too
* MoonSharpHidden attribute as a shortcut for MoonSharpVisible(false) also in an easier namespace to use
* UnityAssetScriptLoader has new constructors which don't require reflection use (to ease IL2CPP porting, which is a pain in any case)
* Fixed a weird bug with using the same destination variable twice in a for-iterator statement (while syntax is parsed correctly, behavior is still undefined as it's not really correct Lua)

<a href="https://github.com/xanathar/moonsharp/releases/download/v1.2.1.0/moonsharp_release_1.2.1.0.zip">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 




#### 2015-12-08

MoonSharp 1.1.0.0 has been released!

* New feature: <a href="coroutines.html#preemptive">Preemptive coroutines</a> to limit time spent in scripts while preserving state.
* Added Script parameter to custom converters - this might break compatibility if you were getting custom converters. (Issue #118)
* Added methods to easily build an array table from a DynValue array.
* Fixed issue #117 - long empty comments not lexed correctly.

<a href="https://github.com/xanathar/moonsharp/releases/download/v1.1.0.0/moonsharp_release_1.1.0.0.zip">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 


#### 2015-10-22

There comes a time when you look at your child and you have to say.. ok you are an adult now!
Ok, not exactly like this, but I felt it was about time to call this a 1.0 !

So, MoonSharp 1.0.0.0 has been released, with a couple of new features and some bug fixes:

* Added a PropertyTableAssigner facility which allows POCO objects made of properties to be filled by deserialization of a table.
* Added a DebuggerEnabled property so that the debugger can be disabled for specific scripts invocations (issue #113)
* Script loading now uses an access mode which allows shared operations (thanks Atom0s)
* Fixed: Capturing varargs in table from inner scope causes null ref exception (issue #110)
* Fixed: Event re-registration might now work (issue #112)
* Fixed: error messages in load and loadfile functions were at times unhelpful (issue #107)

Also in the news : I'm working on Unity asset store packages. Stay tuned.

<a href="https://github.com/xanathar/moonsharp/releases/download/v1.0.0.0/moonsharp_release_1.0.0.0.zip">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 


#### 2015-07-09

MoonSharp 0.9.8.0 released with mostly bugfixes.

* Fixed a major bug in how variable arguments were handled (issue #92)
* Fixed a problem with the registration of extension types (issue #100)
* Fixed a potential incompatibility with IronPython and assembly auto-discovery (issue #96)
* Fixed an invalid datetime format specifier in os module (thanks edwingeng) (issue #99)
* Ownerships checks added to scripts, tables and closures (issue #93)

<a href="https://github.com/xanathar/moonsharp/releases/download/v0.9.8.0/moonsharp_0.9.8.0.zip">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 



#### 2015-06-15

A small, bug fix release has been released - 0.9.6.2 :

* Fixed a bunch of bugs concerning tuples and variable arguments (``...``) functions (See issue #92)
* Added checks for illegal cross-script usage of some types (See issue #93)

<a href="https://github.com/xanathar/moonsharp/releases/download/0.9.6.2/moonsharp_0.9.6.2.zip">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 


#### 2015-05-22

Not even two days since 0.9.4 release, and 0.9.6 is out.

> With this, MoonSharp goes on hold for a 2-3 weeks for personal reasons (new job..) and focus will be on updating the documentation.

Changes:

* Support for registration of generic type definitions (e.g. ``List<>``)
* Support for registration of generic extension methods (beware using this on iOS)
* Support for registered multidimensional arrays
* Support for user-defined member descriptors
* Support for calling members with ``__call`` metamethod through a ``Script.Call`` method
* Revised support for registered array types (was breaking on some Mono versions)
* Fixed: indexers not working on multiple base type dispatch

<a href="https://github.com/xanathar/moonsharp/releases/download/0.9.6/moonsharp_0.9.6.zip">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 

#### 2015-05-20

MoonSharp 0.9.4 has been released, with a lot of new features, mostly revolving around interop with userdata with elements which could not be 
interacted with before (events, enums, vararg functions, const and readonly members, etc.).

**Documentation and tutorials will be updated in the next few days to reflect the latest changes!**.

Changes: 

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


<a href="https://github.com/xanathar/moonsharp/releases/download/0.9.4/moonsharp_0.9.4.zip">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 




#### 2015-03-31

MoonSharp 0.9.2 has been released, with a lot of new features, mostly revolving around interop with userdata (C# classes).

Changes:

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

<a href="https://github.com/xanathar/moonsharp/releases/download/0.9.2/moonsharp_release_0.9.2.zip">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 



#### 2015-03-24

MoonSharp 0.9.0 has been released, with a lot of new features, including support for new platforms.

As the release is **BIG** it will take a few days to update the tutorials on the website.
The reference, on the other hand, is already up to date.

Changes:

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


<a href="https://github.com/xanathar/moonsharp/releases/tag/v0.9.0">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 






#### Older news...

See in the <a href="archive.html">archive page</a>.

