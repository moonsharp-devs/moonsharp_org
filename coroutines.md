---
layout: tutorial
title: Coroutines
subtitle: From both C# and Lua.
---


#### Coroutines in Lua

Coroutines in Lua are supported out of the box. Really, as long as you don't exclude the coroutines module intentionally (see sandboxing), they are supported for free.
There are a lot of caveats (which incidentally apply also to the original Lua implementation) and are discussed in the "Caveats" section below.

Use any Lua coroutine tutorial to work with them.


#### Coroutines from CLR code

Coroutines can be created with the script *CreateCoroutine* method, which accepts a DynValue which must be a function. 

{% highlight csharp %}

string code = @"
	return function()
		local x = 0
		while true do
			x = x + 1
			coroutine.yield(x)
		end
	end
	";

// Load the code and get the returned function
Script script = new Script();
DynValue function = script.DoString(code);

// Create the coroutine in C#
DynValue coroutine = script.CreateCoroutine(function);

// Resume the coroutine forever and ever..
while (true)
{
	DynValue x = coroutine.Coroutine.Resume();
	Console.WriteLine("{0}", x);
}

{% endhighlight %}


#### Caveats

And here we come to the most important section of all the coroutines stuff.
Just like in the original Lua, it's not possible to yield out of nested calls.

In particular, in MoonSharp if you *Call* a script inside a C# function called from Lua, you can't use yield to resume to a coroutine external to the C# call.

There is a way out: returning a *TailCallRequest* DynValue:


{% highlight csharp %}

return DynValue.NewTailCallReq(luafunction, arg1, arg2...); 

{% endhighlight %}


It's also possible to specify a continuation - a piece of function which will be called **after** the execution of the tail call completes.

99% of the time this is probably overkill - not even the Lua standard library handles callbacks+yield correctly in the vast majority of the cases. But 
if you plan to implement APIs like *load*, *pcall* or *coroutine.resume* by yourself, this is needed.

> As an aside, in some corner cases MoonSharp handles yielding in a different way (better in every case I tried so far but who knows) than standard Lua. For example, tostring() supports yielding __tostring metamethods without raising an error.

