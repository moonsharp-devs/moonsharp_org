---
layout: tutorial
title: Script options
subtitle: How to tweak scripts behavior
---

The ``Script`` object provides a few options to tweak its behavior.

There are two kinds of options: local options and global options.


#### Local options

Local options can have a different value for each instance of the ``Script`` object. 
There is a ``Script.DefaultOptions`` static variable which holds the defaults; the defaults are copied over ``script.Options`` everytime a new ``Script`` is created, so that they can be customized further.


<div class="table-responsive">
	<table class="table table-striped table-condensed">
		<tr>
			<th>ScriptLoader</th>
			<td>This is the <code>IScriptLoader</code> for this script. See the section on script loaders for more details.</td>
		</tr>
		<tr>
			<th>DebugPrint</th>
			<td>This is a <code>Action&lt;string&gt;</code> delegate which will be called everytime <code>print</code> is called by Lua code.</td>
		</tr>
		<tr>
			<th>DebugInput</th>
			<td>This is a <code>Func&lt;string, string&gt;</code> delegate which will be called everytime MoonSharp requires line-input from a console, for example in the case of <code>debug.debug</code>.</td>
		</tr>
		<tr>
			<th>UseLuaErrorLocations</th>
			<td>If this is set to true, locations in error messages will only include the line numbers instead of lines and columns. Use this for compatibility with legacy Lua code which parses error messages.</td>
		</tr>
		<tr>
			<th>Stdin, Stdout and Stderr</th>
			<td>Default streams to be used in <code>io</code>/<code>file</code> functions.</td>
		</tr>
		<tr>
			<th>TailCallOptimizationThreshold</th>
			<td>This is quite a complex option. Basically Lua uses a technique to drammatically reduce stack usage in certain scenarios at the expense of simplicity and 
			ease of debugging. MoonSharp can behave the same way, but by default doesn't until it's needed. This defines the threshold. Refer to the reference help for more details.</td>
		</tr>
		<tr>
			<th>CheckThreadAccess</th>
			<td>Again a complex and unusual option. MoonSharp does some sanity checks to prevent scripts being used by multiple threads concurrently. For reasons detailed in the reference help, these checks can have false positives and false negatives. If you are experiencing false positives, you can disable the checks with this option. But please check that you are not calling MoonSharp execution concurrently from two threads as it is not supported.</td>
		</tr>
	</table>
</div>



#### A quick example of overridden script options

In this example we will override how text is printed, first globally, then specifically on our script:


{% highlight csharp %}
static void OverriddenPrint()
{
	// redefine print to print in lowercase, for all new scripts
	Script.DefaultOptions.DebugPrint = s => Console.WriteLine(s.ToLower());
		
	Script script = new Script();

	DynValue fn = script.LoadString("print 'Hello, World!'");

	fn.Function.Call(); // this prints "hello, world!"

	// redefine print to print in UPPERCASE, for this script only
	script.Options.DebugPrint = s => Console.WriteLine(s.ToUpper());
	
	fn.Function.Call(); // this prints "HELLO, WORLD!"
}
{% endhighlight %}



#### Global Options

As said, global options are accessed through ``Script.GlobalOptions`` and are global to all scripts. 

At the moment there are two options:

* ``Platform`` : the platform accessor we learned about in the previous chapter
* ``CustomConverters`` : the collection of all custom converters

As all global options have been touched in previous chapters, we will not spend any further time on them. 



