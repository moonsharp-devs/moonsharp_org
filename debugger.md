---
layout: tutorial
title: Debugger integration
subtitle: How to offer a debugger to your users. Or yourself.
---


#### Use the available remote debugger

99.9% of the time, if you want to offer a debugger to your users, you can just go with using the embedded remote debugger.

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
* Please refer to the current existing implementation and reference help for details













        
		
		
		


