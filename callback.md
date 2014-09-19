---
layout: tutorial
title: Calling back C#
subtitle: .. but also F# and VB.NET and C++/CLI and Boo and..
---

Scripts are useful in applications because they can customize the business logic starting from building blocks implemented in the containing application itself.
Whether you are embedding scripts in a business application, a videogame or some kind of tool, your first concern is interoperability of script and application
and sharing (in a sense) of objects between the twos.

And the basic building blocks in procedural programming are functions.


#### Excercise 1, Step 1: never get tired of factorials

Let's catch base with the standard factorial script we used so far:


{% highlight csharp %}

private static double CallbackTest()
{
	string scriptCode = @"    
		-- defines a factorial function
		function fact (n)
			if (n == 0) then
				return 1
			else
				return n * fact(n - 1);
			end
		end";

	Script script = new Script();

	script.DoString(scriptCode);

	script.Globals["Mul"] = (Func<int, int, int>)Mul;

	DynValue res = script.Call(script.Globals["fact"], 4);

	return res.Number;
}

{% endhighlight %}


#### Excercise 1, Step 2: Customize the multiplication function

Ok, so let's suppose we want the multiplication to be implemented in an API function of the hosting application.
In this case there's obviously no purpose in doing so, but we are here to learn, so let's pretend it's a good idea.

{% highlight csharp %}

private static int Mul(int a, int b)
{
	return a * b;
}

private static double CallbackTest()
{
	string scriptCode = @"    
		-- defines a factorial function
		function fact (n)
			if (n == 0) then
				return 1
			else
				return Mul(n, fact(n - 1));
			end
		end";

	Script script = new Script();

	script.DoString(scriptCode);

	script.Globals["Mul"] = (Func<int, int, int>)Mul;

	DynValue res = script.Call(script.Globals["fact"], 4);

	return res.Number;
}

{% endhighlight %}

That's it! 

{% highlight csharp %}
script.Globals["Mul"] = (Func<int, int, int>)Mul;
{% endhighlight %}

This sets the "Mul" variable in the global environment to point to the Mul delegate of the application. 
We are casting the delegate to it's type of Func<int, int, int> to please the C# compiler here - whatever technique you use to cast the delegate to a System.Object is ok.

Also, note that we defined the methods to be static, but in this case there's no need at all for them to be; instance methods are good to go.


#### Exercise 2: Returning a sequence of numbers

Problem: have an API function which returns a sequence of integers. The script will sum the numbers received and return the total.

{% highlight csharp %}

private static IEnumerable<int> GetNumbers()
{
	for (int i = 1; i <= 10; i++)
		yield return i;
}

private static double EnumerableTest()
{
	string scriptCode = @"    
		total = 0;
		
		for i in getNumbers() do
			total = total + i;
		end

		return total;
	";

	Script script = new Script();

	script.Globals["getNumbers"] = (Func<IEnumerable<int>>)GetNumbers;

	DynValue res = script.DoString(scriptCode);

	return res.Number;
}

{% endhighlight %}

Here, too, nothing too difficult. You can see how an IEnumerable (or IEnumerator) is converted on the fly to a Lua iterator for the script to run.

Note also how the line 

{% highlight csharp %}
DynValue res = script.DoString(scriptCode);
{% endhighlight %}

is now written **after** we set up the global environment. This is because the scripts contains directly executable code and thus accesses the getNumbers method
immediately.




<h1>To be completed...</h1>




