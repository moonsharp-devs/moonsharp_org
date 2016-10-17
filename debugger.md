---
layout: tutorial
title: Debugger integration
subtitle: How to offer a debugger to your users. Or yourself.
---


#### Use one of the available debuggers

MoonSharp offers, out of the box, two debuggers.

* A debugger which allows attaching from <a href="https://code.visualstudio.com/">Visual Studio Code</a>
* A remote debugger running on any browser supporting Flash (including Chrome)

99.9% of the time, if you want to offer a debugger to your users, you can just go with using one of those two.

Overall, the VSCode debugger is friendlier and easier to use, but it requires the application running MoonSharp to be running on the same machine where the VSCode editor is running, so it's limited to desktop, local platforms. 


#### Using the Visual Studio Code based debugger

To use it, you have to reference ``MoonSharp.VsCodeDebugger.dll`` either manually or through <a href="https://www.nuget.org/packages/MoonSharp.Debugger.VsCode/">NuGet</a>.
If you use the Unity package, it's already included. 


<div class="alert alert-info" role="alert">
NOTE : Your users will have to install the <a href="https://marketplace.visualstudio.com/items?itemName=xanathar.moonsharp-debug">MoonSharp Debug extension for VsCode.</a>

Unlike other debug extensions, The MoonSharp VSCode debugger extension supports commands from the Debug Console window. Type ``!help`` to get a list of available commands.
</div>

Once you have the DLLs referenced, setting it up for a first use is very very simple. 
As simple as:

{% highlight csharp %}

// Create the debugger server
MoonSharpVsCodeDebugServer server = new MoonSharpVsCodeDebugServer();
// Start the debugger server
server.Start();

// Create a script
Script script = new Script();
// Attach the script to the debugger
server.AttachToScript(script1, "Name for the script");

{% endhighlight %}

##### Using the VSCode debugger with multiple script

You can attach multiple scripts to the VSCode debugger without any constraints - although, the more you add, the more you should use meaningful names to avoid confusing your users.

You can switch the script which will be debugged first when a new debugger client is attached by setting the ``Current`` property on the ``MoonSharpVsCodeDebugServer`` object; users can switch the script from the VsCode Debug Console, in any case, using the ``!list`` and ``!switch`` commands.

If a script becomes obsolete and there is no reason to debug it anymore, call the ``Detach`` method on the ``MoonSharpVsCodeDebugServer`` object; this will disconnect any debugger currently debugging that obsolete script object, allowing memory to be freed up and avoiding polluting the script list. 


##### Mapping VSCode debugger to file system files

VSCode debugging relies on the usage of files on the filesystem. If a source file cannot be found, a temporary file is saved on the fly on the local machine with the contents of the script. This for examples happens if the directory where the files are located is different from the relative path used by the application to load, or if the scripts are embedded resources of some kind, or if they are loaded using ``LoadString`` even if they are files.

Luckily ``MoonSharpVsCodeDebugServer`` accepts a third parameter in the ``AttachToScript`` method, which is a function mapping a ``SourceCode`` object to a filename. For example, this piece of code uses ``LoadFile`` to load relative source code, but is still compatible with the debugger:


{% highlight csharp %}
var server = new MoonSharpVsCodeDebugServer().Start();
var script  = new Script();

server.AttachToScript(script, "My script", s => "/temp/lua/" + s.Name);

script.DoFile("fact.lua");

{% endhighlight %}

Another example, this uses the friendlyName parameter of LoadString to avoid the problem altogether:

{% highlight csharp %}
var server = new MoonSharpVsCodeDebugServer().Start();
var script  = new Script();

server.AttachToScript(script, "My script");

script.DoString(File.ReadAllText(@"/temp/lua/fact.lua"), null, @"/temp/lua/fact.lua");
{% endhighlight %}



#### Using the Flash based remote debugger

To use it, you have to reference ``MoonSharp.RemoteDebugger.dll`` either manually or through <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">NuGet</a>.

After that, setting it up for a first use is very very simple. Simple as:


{% highlight csharp %}

RemoteDebuggerService remoteDebugger;

private void ActivateRemoteDebugger(Script script)
{
	if (remoteDebugger == null)
	{
		remoteDebugger = new RemoteDebuggerService();
		
		// the last boolean is to specify if the script is free to run 
		// after attachment, defaults to false
		remoteDebugger.Attach(script, "Description of the script", false);
	}
	
	// start the web-browser at the correct url. Replace this or just
	// pass the url to the user in some way.
	Process.Start(remoteDebugger.HttpUrlStringLocalHost);
}

{% endhighlight %}


> The remote debugger uses Flash, which is very picky on how the domain name is expressed in the URL bar. For example, ``localhost`` and ``127.0.0.1`` are considered different
> things for Flash security. If something doesn't work, try to express the host name part of the URL in a different way.



#### Customize the remote debugger

Some simple customizations are available with the remote debugger. They are passed to the RemoteDebuggerService constructor, through a RemoteDebuggerOptions object.

The following are available:

* **NetworkOptions** : Flags. ``Utf8TcpServerOptions.LocalHostOnly``, specifies that only connections from the local host are accepted. ``Utf8TcpServerOptions.SingleClientOnly`` specifies that at most one client can be connected (at a connection, all previous clients are force disconnected).
* **SingleScriptMode** : if true, the entry web page is skipped and the debugger directly opens. Usable only for programs where a single ``Script`` object is instantiated at a time.
* **HttpPort** : the port at which the HTTP connection is open.
* **RpcPortBase** : the port at which commands for the first ``Script`` objects are received. Every other script increments from this number.

The defaults are:

{% highlight csharp %}

return new RemoteDebuggerOptions()
{
	NetworkOptions = Utf8TcpServerOptions.LocalHostOnly | Utf8TcpServerOptions.SingleClientOnly,
	SingleScriptMode = false,
	HttpPort = 2705,
	RpcPortBase = 2006,
};

{% endhighlight %}


#### Implementing your own debugger

This is a very advanced topic.

It's also possible to implement your own debugger. We won't dig it here too much but:

* A debugger implements the ``MoonSharp.Interpreter.Debugging.IDebugger`` interface.
* All methods of the interface are called, after a debugger has been attached, by the script engine itself
* Please refer to the current existing implementations and reference help for details













        
		
		
		


