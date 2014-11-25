---
layout: home
---

Don't know what MoonSharp is ? <a href="about.html" class="alert-link">Read here.</a>.

Latest release: <a href="changelog.html" class="alert-link">0.7.0</a>.

Next scheduled release: <a href="https://github.com/xanathar/moonsharp/milestones"  class="alert-link">Still unknown.</a>

<hr />

#### 2014-11-25

Work is going on on MoonSharp. A lot of things are open actually:

* Tutorials for debugger integration
* Unity Web Player support
* os, io and file standard libraries

On a negative side, MoonSharp has applied for an open-source license of Xamarin (technically MoonSharp respects their acceptance criteria, AFAICS) but not news from them yet. Given the situation,
references to Xamarin on this site have been removed, as compatibility cannot be guaranteed. Hopefully our voices will be heard.

<hr />

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

<hr />

#### 2014-11-06

Work on the debugger is almost complete!

You can download a <a href="demo/MoonSharpDebuggerDemo.zip">little demo here</a>, a quick and dirty tic-tac-toe game where the AI is implemented in Lua and is completely debuggable.

Otherwise you can peek at a <a href="images/MoonSharpDebugger.png" target="_blank">screenshot of the debugger</a>.

How it works ? The challenge was to give a UI to a DLL which can be used in very different scenarios: GUI applications, full screen games, standalone services etc.

So the debugging is remote, and upon activation of the debugger features, any web browser can be used to debug remotely!. As security measure, by default the debugger
is enabled only for localhost, but it can be enabled for network debugging too. As now the debugger application itself is written in Flex/Flash, so a Flash player is required.

Happy debugging!

<hr />

#### 2014-11-05

MoonSharp has a new logo, thanks to Isaac, a friend in the Grimrock modding community!

On more techincal things.. as a peek of new functionality: a dynamic expression facility has been added.
In this way, .NET code may evaluate expressions without compiling Lua bytecode using the current
execution context, and the same can be done by Lua code.

The evaluation will always perform raw access and will never call any function - it can be used to implement
debuggers or things like a data load from a table source (using tables as if it was a format like JSON.. except Lua).

The evaluation is totally dynamic. So for example, if the expression contains a reference to a variable "x", and we are inside a whatsoever function, it might evaluate as a global. Reevaluate the same expression when a function with an upvalue (or local, or argument) "x" is on the stack, and "x" will refer to that upvalue/local/argument!

<hr />

#### 2014-11-04

Version 0.7.0 will be probably out in a week, but don't hold your breath as it will be the biggest MoonSharp release so far, so the probability of it slipping is high.

Hopefully I will be able to release MoonSharp 0.7.0 before the week of Nov 17th, otherwise it will slip for another week or two.

That week I'll be attending O'Reilly Velocity Europe 2014 in Barcelona.. if anyone is there and wants to talk of MoonSharp things, let me know, at info@moonsharp.org.

<hr />

#### 2014-10-21

Minor changes have been done to how escape sequences in strings are managed.

Also, the sharing objects and error handling tutorials have been added!

<hr />

#### 2014-10-20

Version 0.6.0 has been released. This is a very big release and contains - above all - a complete string library and a vastly
improved error reporting for both syntax and runtime errors. <a href="changelog.html" class="alert-link">See the changelog</a> for 
a complete list of changes.

Note: there are a couple of compatibility breaks in this version - nothing that can't be fixed in 5 minutes, but you've been 
warned.

<hr />

#### 2014-10-17

The string library has been completed. 
For the sake of not reinventing the wheel (or, at least, not reinventing *every* wheel), it was ported from KopiLua code.
In the process, a compatibility layer has been implemented to ease the porting of code using classic stack-based APIs like
KopiLua, UniLua or Lua itself.

On a side note, visit <a href="http://www.darkdale.net" class="alert-link">www.darkdale.net</a>. It's the project of a friend, 
a dungeon crawler on the spirit of Eye of the Beholder, Lands of Lore and Legend of Grimrock and it uses MoonSharp as a 
scripting engine!

<hr />

#### 2014-10-08

MoonSharp 0.5.5 has been released. 

* Optimized parsing stage
* Performance statistics for diagnostics



