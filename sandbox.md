---
layout: tutorial
title: Sandboxing
subtitle: Restricting what Lua script can do
---

#### Why sandboxing ?

More likely than not, sandboxing is a key feature in your MoonSharp usage. Unless you control, in some way, the script providers (and maybe also the users), there is a fundamental trust problem in loading scripts (or any kind of code and sometimes even data) at runtime: security.

For example, let's say that you are writing a videogame and you use Lua to make the game "moddable". Users can go to some community website and download a mod containing 
new levels, AIs and what-not all scripted using MoonSharp for the maximum flexibility. Everybody is happy. Now think of what would happen to your game's reputation if some
evil user uploads some malicious scripts well hidden in some beautiful level.. like a script creating an .exe file on the desktop, where other users are likely to click 
accidentally. 

You don't want to expose some APIs (like 'os', 'io' and 'file') to the general users unless either you trust them or it's plainly evident that malicious use is possible.


#### A Sandboxing checklist

This checklist is something to get you started, but it's not comprehensive.

* Check that you made all "dangerous" standard APIs inaccessibile to scripts. This include 'io', 'os' and 'file' for sure, but depending on your scripts and your level of paranoia more could be included. See the next chapter to see how to make whole sets of APIs inaccessible in an easy way.

* Do *NOT* use ``InteropRegistrationPolicy.Automatic``. Ever.

* Check that you are not exposing to scripts types, or member of types, which shouldn't be exposed. Avoid exposing directly types you don't own, like .NET framework types or Unity types. If needed, check the section about "proxy objects" and how to use ``MoonSharpHide`` and ``MoonSharpHidden``.

* If you want parts of your scripts to access some of these dangerous APIs, keep them in separate Script objects and ensure no access to those objects is given to other scripts.

* If you absolutely must "sandbox" only a part of your script, refer to <a href="http://lua-users.org/wiki/SandBoxes">this evergreen guide</a>. 


#### Removing "dangerous" APIs 

The easiest way to remove dangerous APIs is to use the constructor of ``Script`` which accepts a ``CoreModules`` enumeration.

The ``CoreModules`` enumeration is pretty self-explanatory:

{% highlight csharp %}

/// <summary>
/// Enumeration (combinable as flags) of all the standard library modules
/// </summary>
[Flags]
public enum CoreModules
{
	/// <summary>
	/// Value used to specify no modules to be loaded (equals 0).
	/// </summary>
	None = 0,

	/// <summary>
	/// The basic methods. Includes "assert", "collectgarbage", "error", "print", "select", "type", "tonumber" and "tostring".
	/// </summary>
	Basic = 0x40,
	/// <summary>
	/// The global constants: "_G", "_VERSION" and "_MOONSHARP".
	/// </summary>
	GlobalConsts = 0x1,
	/// <summary>
	/// The table iterators: "next", "ipairs" and "pairs".
	/// </summary>
	TableIterators = 0x2,
	/// <summary>
	/// The metatable methods : "setmetatable", "getmetatable", "rawset", "rawget", "rawequal" and "rawlen".
	/// </summary>
	Metatables = 0x4,
	/// <summary>
	/// The string package
	/// </summary>
	String = 0x8,
	/// <summary>
	/// The load methods: "load", "loadsafe", "loadfile", "loadfilesafe", "dofile" and "require"
	/// </summary>
	LoadMethods = 0x10,
	/// <summary>
	/// The table package 
	/// </summary>
	Table = 0x20,
	/// <summary>
	/// The error handling methods: "pcall" and "xpcall"
	/// </summary>
	ErrorHandling = 0x80,
	/// <summary>
	/// The math package
	/// </summary>
	Math = 0x100,
	/// <summary>
	/// The coroutine package
	/// </summary>
	Coroutine = 0x200,
	/// <summary>
	/// The bit32 package
	/// </summary>
	Bit32 = 0x400,
	/// <summary>
	/// The time methods of the "os" package: "clock", "difftime", "date" and "time"
	/// </summary>
	OS_Time = 0x800,
	/// <summary>
	/// The methods of "os" package excluding those listed for OS_Time. These are not supported under Unity.
	/// </summary>
	OS_System = 0x1000,
	/// <summary>
	/// The methods of "io" and "file" packages. These are not supported under Unity.
	/// </summary>
	IO = 0x2000,
	/// <summary>
	/// The "debug" package (it has limited support)
	/// </summary>
	Debug = 0x4000,
	/// <summary>
	/// The "dynamic" package (introduced by MoonSharp).
	/// </summary>
	Dynamic = 0x8000,


	/// <summary>
	/// A sort of "hard" sandbox preset, including string, math, table, bit32 packages, constants and table iterators.
	/// </summary>
	Preset_HardSandbox = GlobalConsts | TableIterators | String | Table | Basic | Math | Bit32,
	/// <summary>
	/// A softer sandbox preset, adding metatables support, error handling, coroutine, time functions and dynamic evaluations.
	/// </summary>
	Preset_SoftSandbox = Preset_HardSandbox | Metatables | ErrorHandling | Coroutine | OS_Time | Dynamic,
	/// <summary>
	/// The default preset. Includes everything except "debug" as now.
	/// Beware that using this preset allows scripts unlimited access to the system.
	/// </summary>
	Preset_Default = Preset_SoftSandbox | LoadMethods | OS_System | IO,
	/// <summary>
	/// The complete package.
	/// Beware that using this preset allows scripts unlimited access to the system.
	/// </summary>
	Preset_Complete = Preset_Default | Debug,

}

{% endhighlight %}





