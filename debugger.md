---
layout: tutorial
title: Debugger integration
subtitle: How to offer a debugger to your users. Or yourself.
---


#### Use the available remote debugger

99.9% of the time, if you want to offer a debugger to your users, you can just go with using the embedded remote debugger.

To use it, you have to reference MoonSharp.RemoteDebugger.dll either manually or through <a href="https://www.nuget.org/packages/MoonSharp.Debugger/">NuGet</a>.

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


#### Customize the remote debugger

Some simple customizations are available with the remote debugger. They are passed to the RemoteDebuggerService constructor, through a RemoteDebuggerOptions object.

The following are available:

* **NetworkOptions** : Flags. Utf8TcpServerOptions.LocalHostOnly, specifies that only connections from the local host are accepted. Utf8TcpServerOptions.SingleClientOnly specifies that at most one client can be connected (at a connection, all previous clients are force disconnected).
* **SingleScriptMode** : if true, the entry web page is skipped and the debugger directly opens. Usable only for programs where a single Script object is instantiated at a time.
* **HttpPort** : the port at which the HTTP connection is open.
* **RpcPortBase** : the port at which commands for the first Script objects are received. Every other script increments from this number.

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

* A debugger implements the MoonSharp.Interpreter.Debugging.IDebugger interface.
* All methods of the interface are called, after a debugger has been attached, by the script engine itself
* Please refer to the current existing implementation for details

The interface currently is:

{% highlight csharp %}

public interface IDebugger
{
	/// <summary>
	/// Called by the script engine  when a source code is added or changed.
	/// </summary>
	/// <param name="sourceCode">The source code object.</param>
	void SetSourceCode(SourceCode sourceCode);
	/// <summary>
	/// Called by the script engine  when the bytecode changes.
	/// </summary>
	/// <param name="byteCode">The bytecode source</param>
	void SetByteCode(string[] byteCode);
	/// <summary>
	/// Called by the script engine at execution time to check if a break has 
	/// been requested. Should return pretty fast as it's called A LOT.
	/// </summary>
	bool IsPauseRequested();
	/// <summary>
	/// Called by the script engine when a runtime error occurs. 
	/// The debugger can return true to signal the engine that it wants to break 
	/// into the source of the error. If it does so, it should also return true 
	/// to subsequent calls to IsPauseRequested().
	/// </summary>
	/// <param name="ex">The runtime exception.</param>
	/// <returns>True if this error should break execution.</returns>
	bool SignalRuntimeException(ScriptRuntimeException ex);
	/// <summary>
	/// Called by the script engine to get what action to do next.
	/// </summary>
	/// <param name="ip">The instruction pointer in bytecode.</param>
	/// <param name="sourceref">The source reference.</param>
	/// <returns>T</returns>
	DebuggerAction GetAction(int ip, SourceRef sourceref);
	/// <summary>
	/// Called by the script engine when the execution ends.
	/// </summary>
	void SignalExecutionEnded();
	/// <summary>
	/// Called by the script engine to update watches of a given type. Note 
	/// that this method is not called only for watches in the strictest term, 
	/// but also for the stack, etc.
	/// </summary>
	/// <param name="watchType">Type of the watch.</param>
	/// <param name="items">The items.</param>
	void Update(WatchType watchType, IEnumerable<WatchItem> items);
	/// <summary>
	/// Called by the script engine to get which expressions are active 
	/// watches in the debugger.
	/// </summary>
	/// <returns>A list of watches</returns>
	List<DynamicExpression> GetWatchItems();
	/// <summary>
	/// Called by the script engine to refresh the breakpoint list.
	/// </summary>
	void RefreshBreakpoints(IEnumerable<SourceRef> refs);
}

{% endhighlight %}












        
		
		
		


