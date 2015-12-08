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

#### Coroutines from CLR code as CLR iterator

It's possible to invoke coroutines as if they were an iterator:

{% highlight csharp %}

string code = @"
	return function()
		local x = 0
		while true do
			x = x + 1
			coroutine.yield(x)
			if (x > 5) then
				return 7
			end
		end
	end
	";

// Load the code and get the returned function
Script script = new Script();
DynValue function = script.DoString(code);

// Create the coroutine in C#
DynValue coroutine = script.CreateCoroutine(function);

// Loop the coroutine 
string ret = "";

foreach (DynValue x in coroutine.Coroutine.AsTypedEnumerable())
{
	ret = ret + x.ToString();
}

Assert.AreEqual("1234567", ret);

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



<a name="preemptive"></a>

#### Preemptive coroutines

In MoonSharp it's possible to have a coroutine suspended even if it does not call ``coroutine.yield``.
This can be useful, for example, if one wants to non-destructively limit the amount of CPU time dedicated to a script, but there are some caveats which are important to maintain script consistency.

Let's start with an example:

{% highlight csharp %}

string code = @"
	function fib(n)
		if (n == 0 or n == 1) then
			return 1;
		else
			return fib(n - 1) + fib(n - 2);
		end
	end
	";

// Load the code and get the returned function
Script script = new Script(CoreModules.None);
script.DoString(code);

// get the function
DynValue function = script.Globals.Get("fib");

// Create the coroutine in C#
DynValue coroutine = script.CreateCoroutine(function);

// Set the automatic yield counter every 10 instructions. 
// 10 is likely too small! Use a much bigger value in your code to avoid interrupting too often!
coroutine.Coroutine.AutoYieldCounter = 10;

int cycles = 0;
DynValue result = null;

// Cycle until we get that the coroutine has returned something useful and not an automatic yield..
for (result = coroutine.Coroutine.Resume(8); 
	result.Type == DataType.YieldRequest;
	result = coroutine.Coroutine.Resume()) 
{
	cycles += 1;
}

// Check the values of the operation
Assert.AreEqual(DataType.Number, result.Type);
Assert.AreEqual(34, result.Number);

{% endhighlight %}

The steps are simple:

* Create a coroutine
* Set the ``AutoYieldCounter`` to some number greater than 0. 1000 is a good starting point, tune from there. This is the number of instructions which will be executed before yielding to the caller.
* Call ``coroutine.Coroutine.Resume(...)`` with the proper args
* If the result of the above call is of type ``DataType.YieldRequest``, call ``coroutine.Coroutine.Resume()`` with no args.
* Repeat the previous until we have a real return type.
* Done.

Now on to the caveats:

* If the code re-enters (for example, the callback of string.gsub), it will not be preempted until it returns to a "yieldable state"
* Mixing standard coroutine operations with preempted ones is possible but dangerous and difficult. Remember that the coroutine can preempt-yield in any place, thus, for example, there are no guarantee of operation atomicity etc. If you mix them, be ware.



