---
layout: tutorial
title: Keeping a Script around
subtitle: Verba volant, scripts manent
---

In the previous tutorial, you got your first contact with MoonSharp: you put a script in a string, run the code and took its 
output.
While that might occasionally be useful, the interoperability needed for most use cases require a little bit more integration
between CLR code and MoonSharp.



#### Step 1: redo all you did in the previous tutorial

Seriously, while we are learning (a little more) advanced concepts here, there's almost nothing to throw away of your first attempt.
And that is a great starting point.

#### Step 2: Change the code to create a Script object

The first change is to create a Script object instead of using one of the static methods.. not rocket science, but it sets us up
for the next evolutions.

{% highlight csharp %}

double MoonSharpFactorial()
{
	string scriptCode = @"    
		-- defines a factorial function
		function fact (n)
			if (n == 0) then
				return 1
			else
				return n*fact(n - 1)
			end
		end

		return fact(5)";

	Script script = new Script();
	DynValue res = script.DoString(scriptCode);
	return res.Number;
}

{% endhighlight %}

#### Step 2: Access the global environment

Now that we have a script object, we can change the global environment in which functions operate. For example, we might want to change the input to the ``fact`` function
so that our program can tell which number to calculate the factorial of.

{% highlight csharp %}

double MoonSharpFactorial()
{
	string scriptCode = @"    
		-- defines a factorial function
		function fact (n)
			if (n == 0) then
				return 1
			else
				return n*fact(n - 1)
			end
		end

		return fact(mynumber)";

	Script script = new Script();

	script.Globals["mynumber"] = 7;

	DynValue res = script.DoString(scriptCode);
	return res.Number;
}

{% endhighlight %}

As you can see, just by referencing the ``script.Globals`` table with a simple syntax, we can inject numbers into MoonSharp scripts. Actually we can pass a lot more
than just numbers, but let's limit ourselves to numbers, booleans and strings for now - we'll see how to pass functions and objects later on.


#### Step 2: Directly calling functions

We learned how to have MoonSharp calculate a factorial of a number chosen from the outside. But well, done like that smells like a dirty hack (still, it's an important technique we'll use a lot).

Here it's how to call a Lua function from C#.

{% highlight csharp %}

double MoonSharpFactorial2()
{
	string scriptCode = @"    
		-- defines a factorial function
		function fact (n)
			if (n == 0) then
				return 1
			else
				return n*fact(n - 1)
			end
		end";

	Script script = new Script();

	script.DoString(scriptCode);

	DynValue res = script.Call(script.Globals["fact"], 4);

	return res.Number;
}

{% endhighlight %}


Let's see what we did.

First of all, we removed the return at the end of the script - we could have kept that, but it was going to be useless as we want to call ``fact`` with parameters of our choice.

At this point, the ``script.DoString(...)`` call will execute the file, leaving a nice ``fact`` function in the global environment.

Then we introduced this line:

{% highlight csharp %}
DynValue res = script.Call(script.Globals["fact"], 4);
{% endhighlight %}

This gets the ``fact`` function from the script globals and then calls it with 4 as a parameter. 

> There are better (as in, more performing) ways to do this, but if you are not doing a lot of calls in a time sensitive loop, this is the simplest possible way, albeit a little type unsafe.



Note that we can call the ``fact`` functions however many times we want - a Script preserves its states and is ready to be executed again and again.





