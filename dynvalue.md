---
layout: tutorial
title: DynValue revealed
subtitle: Everything is a DynValue is a DynValue
---

The DynValue concept is quite at the root of all Moon#, and while we have managed to have little to do with it so far, in fact it's not so feasible to go very far
without touching the subject.

As said in the title, (almost) everything in Moon# is an instance of a DynValue object. A DynValue represents a value in the script, whatever type it has, and so
it can be a table, a function, a number, a string and everything else too.

So, to start, let's start with the latest tutorial step and change it to use DynValue(s) instead.


#### Step 1: redo all you did in the previous tutorial

Again, we start from the last step of the previous one. You did it, right ?

Also keep a tab handy to the [DynValue reference page](reference/html/b4040a8c-9d07-b73a-1789-e316a55e8e49.htm)


#### Step 2: Get the fact function in a DynValue

The first change, we get the *fact* variable in a different way:

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
		end";

	Script script = new Script();

	script.DoString(scriptCode);

	DynValue luaFactFunction = script.Globals.Get("fact");

	DynValue res = script.Call(luaFactFunction, 4);

	return res.Number;
}

{% endhighlight %}

Ok, let's spend some words on this line:

{% highlight csharp %}
DynValue luaFactFunction = script.Globals.Get("fact");
{% endhighlight %}

This gets the *fact* function from the script globals. 
The *Get* method, opposite to the indexer property, does return a DynValue - the indexer, instead, attempts to convert it to a *System.Object*.
In this case we are interested on the DynValue, so we use the Get method. 


#### Step 3: Get the number in a DynValue

Ok, now if we are interested in avoiding yet another conversion, we can also pass directly a number instead of using the implicit conversions the *Call* method gently offers.
Remember, here it's not **needed** but somewhere else, it might be, not everything does magic conversions for you!

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
		end";

	Script script = new Script();

	script.DoString(scriptCode);

	DynValue luaFactFunction = script.Globals.Get("fact");

	DynValue res = script.Call(luaFactFunction, DynValue.NewNumber(4));

	return res.Number;
}

{% endhighlight %}

So, we replaced our number with

{% highlight csharp %}
DynValue.NewNumber(4)
{% endhighlight %}

DynValue has a lot of factory methods, all starting with "New" (like NewString, NewNumber, NewBoolean, etc.) which can be used to build our values from scratch.

It also has a handy *FromObject* method, which creates a DynValue from object and is just what *Call* was using, under the hood, to ease our lives.


#### Step 3: Understanding DataType (s)

One of the most important properties in DynValue is [Type](reference/html/b3642bf3-cb09-67c5-17d4-d36a6c1ef364.htm).

The Type property is an enumeration telling us what kind of data is contained in a DynValue.

We can query the Type property everytime we want to know what is contained in a DynValue:

{% highlight csharp %}
// Create a new number
DynValue v1 = DynValue.NewNumber(1);
// and a new string
DynValue v2 = DynValue.NewString("ciao");
// and another string using the automagic converters
DynValue v3 = DynValue.FromObject(new Script(), "hello");

// This prints Number - String - String
Console.WriteLine("{0} - {1} - {2}", v1.Type, v2.Type, v3.Type);
// This prints Number - String - Some garbage number you shouldn't rely on to be 0
Console.WriteLine("{0} - {1} - {2}", v1.Number, v2.String, v3.Number);
{% endhighlight %}

It's important to know that some properties of DynValue have a meaningful value if and only if the value is of a given type. For example, the Number property is
guaranteed to have a meaningful value only if the type is Number, and the same for the String property.

<div class="alert alert-warning" role="alert">
Always check the Type of a DynValue before using its properties unless you are sure of what it contains!
</div>

#### Step 4: Tuples

As you (should) know, Lua can return multiple values from a function (and in other situations).

To handle this, a special DynValue type (Tuple) is used. A tuple has a Tuple property which is an array of DynValue objects which are the members of the Tuple.

This is simpler than it sounds:

{% highlight csharp %}
DynValue ret = Script.RunString("return true, 'ciao', 2*3");

// prints "Tuple"
Console.WriteLine("{0}", ret.Type);

// Prints:
//   Boolean = true
//   String = "ciao"
//   Number = 6
for (int i = 0; i < ret.Tuple.Length; i++)
	Console.WriteLine("{0} = {1}", ret.Tuple[i].Type, ret.Tuple[i]);
{% endhighlight %}

#### Closing

We are done with DynValues .. there's a lot more to learn, but this should be enough to get you started on the topic.







