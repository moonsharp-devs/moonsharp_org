---
layout: home
---

Don't know what MoonSharp is ? <a href="about.html" class="alert-link">Read here.</a>.

Latest release: <a href="changelog.html" class="alert-link">1.0.0.0</a> \| <a href="https://github.com/xanathar/moonsharp/releases/download/v0.9.8.0/moonsharp_0.9.8.0.zip">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 


Next scheduled release: <a href="https://github.com/xanathar/moonsharp/milestones"  class="alert-link">Who knows.</a> (Bug fix releases may come earlier)


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

<a href="https://github.com/xanathar/moonsharp/releases/download/v0.9.8.0/moonsharp_0.9.8.0.zip">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 


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




#### 2015-03-23

As the release for MoonSharp 0.9.0 is approaching, the documentation and tutorials are being updated,
thus they might be inconsistent with what's available in 0.8.x.

As soon as all the tests on iOS over Unity3D are passed, 0.9.0 will be released!

Check the <a href="changelog.html">CHANGE LOG</a> to have a peek at what's coming.




#### 2015-03-20

Time for some MoonSharp 0.9.0 teasers...

* The test suite <a href="images/Lumia.jpg">running on a Nokia Lumia 620</a>
* The test suite <a href="images/w8.png">running on Windows 8.1 as a Windows 8 Store App</a>
* The test suite <a href="images/sl5.PNG">running on Silverlight 5 on Chrome</a>




#### 2015-03-18

Wooo so many new things!

MoonSharp 0.9.0 is near, with more changes than ever.

Check the <a href="changelog.html">CHANGE LOG</a> to have a peek at what's coming.




#### 2015-03-12

Almost two months of silence! But many things happened in this time frame, even if not a lot of code was committed to the repo.

I did a little summary of major outstanding issues of MoonSharp:

* Incompatible with iOS (and, in general, Mono full AOT) because of how ANTLR handles things with complex generics and delegates (I worked around many of them - in mono aot as I don't have a Mac nor an iPhone - as you can read here: https://groups.google.com/forum/#!topic/moonsharp/oT4SbOFA5mE )
* Some malformed scripts give errors with unhelpful error messages, because of the way ANTLR reports lexer errors
* Some performance issues during lexing/parsing because of the complex ANTLR grammar and analysis. I improved them a lot by removing operator precedence in the grammar and manually handling it on the MoonSharp AST, but some still there.
* Minor lexical problems due to a corner case ambiguity which ANTLR resolves and Lua itself not
* A complex deployment issue. Currently ANLTR 4.3 gives different version of .NET 3.5, .NET 4, .NET 4.5, Portable .NET 4, Portable .NET 4.5 at least. This means a lot of effort on the MoonSharp side to manage builds for Windows 8 apps, Windows Phone, XBox, etc. Solved in ANTLR 4.4 but not yet released.

Sense a pattern ? :)

Now, don't get me wrong, ANTLR is a **wonderful** tool. Simply, I think it makes a lot more sense on DSLs enterprise scenarios rather than generic scripting languages on 3rd party lib.

So, to cut short, an attempt at a custom lexer+parser is ongoing, hopefully leading to a better solution in the long run. But this is a very very long work, happening on the "remove-antlr" branch.





#### 2015-01-13

MoonSharp 0.8.5.1 has been released, with many bugs fixed over 0.8.2.1 and a revised userdata interfacing.

* Vastly improved type descriptors  - #39
* Fixed: Varargs not supported on main chunk  - #46
* Fixed: pcall returns only the first return value  - #47
* Fixed: Threading check not working properly  - #42
* Fixed: Exception ctor overloads revised for potential obscure bugs  - #40
* Fixed: String patterns do not support \0 characters - %z must be used instead.  - #29

In particular userdata type descriptors are improved in this way:

* If a customized behaviour for a given type is desired, it can be registered using a custom IUserDataDescriptor, or the object can implement IUserDataType for an even easier implementation
* While still considered somewhat unsafe, it's possible to use "UserData.RegistrationPolicy = InteropRegistrationPolicy.Automatic" to have unregistered types be registered just in time
* If interfaces are registered and a type implements more than one interface, correct resolution now happens
* Types registered with the standard descriptors support some kind of automatic member name adaptation. For example, a member called "SomeMethodWithLongName" can be accessed from a lua script also as "someMethodWithLongName" or "some_method_with_long_name" (for better consistency among coding styles in different languages).
* Types registered with the standard descriptors and having overloaded methods will not raise an error anymore, but resolve to the first method found (quite random). Better overloads support is a task for the future.


<a href="https://github.com/xanathar/moonsharp/releases/tag/v0.8.5.1">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 




#### 2014-12-24

MoonSharp 0.8.2.1 has been released, with 4 bugs fixed over 0.8.1:

* Function in lua which has 2 parameters with the same name throws error
* UserData with private properties throws on UserData.RegisterType
* table.unpack raises IndexOutOfRangeException
* Assigning nil to a table field should delete it
* Minor/partial fix on the multithread check on script execution

**Note: 0.8.2.1 has not been released on NuGet at the moment**

<a href="https://github.com/xanathar/moonsharp/releases/tag/v0.8.2.1">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 




#### 2014-12-18

MoonSharp 0.8.1 has been released, with two critical bug fixed over 0.8.0:

* A **lot** of bug-fixes regarding error handling on nested C# calls and coroutines
* A bad bug involving a miscalculation of table length has been fixed (See issue #38)

<a href="https://github.com/xanathar/moonsharp/releases/tag/v0.8.1">Zip file</a> \| <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter NuGet</a> \| <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger NuGet</a> 



#### 2014-12-11

MoonSharp 0.8.0 has been released, with many interesting additions, including a completed standard library, some support for Unity
and Xamarin and debugger additions.

* Support for Unity Web Player and Unity Android (non-stripped)
* Support for Xamarin Android
* 'os', 'io' and 'file' libraries completed. Note that in Unity and Xamarin some functionality of 'os', 'io' and 'file' are not supported.
* Parts of the 'debug' library completed. This has been done for compatibility sake, as debuggers are implemented differently in MoonSharp.
* Improved error messages
* Improved options management [small breaking change]
* Table indexers now support any number of keys and resolve to subtables automagically
* Fixed a bug where a piece code behaved differently in .NET 4 than in .NET 2 (weird covariance stuff, this btw is interesting weird corner case).
* Test suite runs on Xamarin Android, Unity Windows, Unity Android and Unity Web Player correctly!
* Remote Debugger: it's now possible to attach the debugger to running scripts without having them getting paused
* Remote Debugger: it's now possible to break on errors, and to select which errors break and which don't
* Remote Debugger: added set breakpoint and clear breakpoint commands

##### Downloads:

* <a href="https://github.com/xanathar/moonsharp/releases/tag/v0.8.0">Download (.zip)</a>
* <a href="https://www.nuget.org/packages/MoonSharp/">Interpreter nuget package</a>
* <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">Debugger nuget package</a>



#### 2014-12-01

Thanks to good feedback on the forums, some facilities have been added to table to ease access to subtables: now the indexers support any number of keys and resolve to subtables automagically!




#### 2014-12-01

Great news:

* MoonSharp has been accepted in Xamarin open source program.. so Xamarin testing will start soon and hopefully a Xamarin package will be released asap!
* xpcall and debug.traceback are in! and working! This basically completes all the work meant to be done to the standard library (actually, it's a lot more than what was meant to be done already!)


#### 2014-11-29

Big news under the standard library side:

* 'file', 'io' and 'os' are supported completely. A tutorial will follow on how to handle binary files as that is a little different from native Lua. Note that only few methods of 'os' are supported on Unity.
* some methods from 'debug' are supported (work still in progress)! This will include debug.debug, debug.[gs]etmetatable, debug.[gs]etupvalue, debug.[gs]etuservalue, debug.getregistry, debug.upvalueid.

Next on backlog for 0.8.0:

* debug.traceback
* xpcall to be completed
* antlr redistribution issues to be solved




#### 2014-11-26

I dedicated some hours lately on getting moonsharp to work on Unity. The test suite has been completely revised and now tests run on Unity too!

Here's some screenshots.. nothing exciting really, but a proof of the work done!

* <a href="images/unityts_ide.png" target="_blank">MoonSharp tests running on Unity IDE</a>
* <a href="images/unityts_web.png" target="_blank">MoonSharp tests running on Unity Web Player</a>
* <a href="images/unityts_s3.png"  target="_blank">MoonSharp tests running on Unity Android / Samsung Galaxy S3</a>

Hope to do a release soon for all you Unity dwellers.

I have not tested other Unity targets as I don't have the devices and neither stripping as I have the free Unity not the Pro ones.. but I'll try to 
get some Apple devices and a Unity trial later to do some tests with stripping enabled.



#### 2014-11-25

Work is going on on MoonSharp. A lot of things are open actually:

* Tutorials for debugger integration
* Unity Web Player support
* os, io and file standard libraries





#### 2014-11-07

Version 0.7.0 has been released!
For people using NuGet, please note that there are now two packages:

* <a href="https://www.nuget.org/packages/MoonSharp/">"MoonSharp" - MoonSharp interpreter</a>
* <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">"MoonSharp.Debugger" - MoonSharp remote debugger</a>

Instructions on how to use the debugger will follow soon... but if you are desperate to try it, add MoonSharp.RemoteDebugger.dll
as a reference (or in NuGet, MoonSharp.Debugger package) and then use this code:

{% highlight csharp %}

RemoteDebuggerService remoteDebugger;

private void ActivateRemoteDebugger(Script script)
{
	if (remoteDebugger == null)
	{
		remoteDebugger = new RemoteDebuggerService();
		remoteDebugger.Attach(script, "Description of the script");
	}
	Process.Start(remoteDebugger.HttpUrlStringLocalHost);
}

{% endhighlight %}

Yep!. It's that simple.



#### 2014-11-06

Work on the debugger is almost complete!

You can download a <a href="demo/MoonSharpDebuggerDemo.zip">little demo here</a>, a quick and dirty tic-tac-toe game where the AI is implemented in Lua and is completely debuggable.

Otherwise you can peek at a <a href="images/MoonSharpDebugger.png" target="_blank">screenshot of the debugger</a>.

How it works ? The challenge was to give a UI to a DLL which can be used in very different scenarios: GUI applications, full screen games, standalone services etc.

So the debugging is remote, and upon activation of the debugger features, any web browser can be used to debug remotely!. As security measure, by default the debugger
is enabled only for localhost, but it can be enabled for network debugging too. As now the debugger application itself is written in Flex/Flash, so a Flash player is required.

Happy debugging!



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



