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

Let's catch base with the standard factorial script we've used so far:


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

    script.Globals["Mul"] = (Func<int, int, int>)Mul;

    script.DoString(scriptCode);

    DynValue res = script.Call(script.Globals["fact"], 4);

    return res.Number;
}

{% endhighlight %}

That's it! 

{% highlight csharp %}
script.Globals["Mul"] = (Func<int, int, int>)Mul;
{% endhighlight %}

This sets the ``Mul`` variable in the global environment to point to the ``Mul`` delegate of the application. 
We are casting the delegate to it's type of ``Func<int, int, int>`` to please the C# compiler here - whatever technique you use to cast the delegate to a ``System.Object`` is ok.

Also, note that we defined the methods to be ``static``, but in this case there's no need at all for them to be; instance methods are good to go.


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

Here, too, nothing too difficult. You can see how an ``IEnumerable`` (or ``IEnumerator``) is converted on the fly to a Lua iterator for the script to run.

Note also how that the scripts contains directly executable code and thus accesses the ``getNumbers`` method
at ``DoString`` time, without needing a ``Call`` .. call. Remember this, ``DoString`` and ``DoFile`` will execute code contained in the script immediately!

> A thing which must be noticed. MoonSharp is able to convert iterators of ``System.Collections.IEnumerable`` and ``System.Collections.IEnumerator`` types.
> That is, the non-generic variants. If for some reason you implement a generic iterator without implementing the non-generic one, the iterator will not work.
> All standard collection types and iterator methods like the above one implement the non-generic variants by default, so there's no need to worry too much.


#### Returning a table

Problem: have an API function which returns a sequence of integers, this time in a table.


{% highlight csharp %}

private static List<int> GetNumberList()
{
    List<int> lst = new List<int>();
    
    for (int i = 1; i <= 10; i++)
        lst.Add(i);

    return lst;
}

private static double TableTest1()
{
    string scriptCode = @"    
        total = 0;

        tbl = getNumbers()
        
        for _, i in ipairs(tbl) do
            total = total + i;
        end

        return total;
    ";

    Script script = new Script();

    script.Globals["getNumbers"] = (Func<List<int>>)GetNumberList;

    DynValue res = script.DoString(scriptCode);

    return res.Number;
}

{% endhighlight %}

Here, we see how a ``List<int>`` gets automatically converted to a Lua table! Note that the resulting table will be 1-indexed as Lua tables usually are. 

We can do better, however. We can directly build a Lua table inside our functions:

{% highlight csharp %}

private static Table GetNumberTable(Script script)
{
    Table tbl = new Table(script);

    for (int i = 1; i <= 10; i++)
        tbl[i] = i;

    return tbl;
}

private static double TableTest2()
{
    string scriptCode = @"    
        total = 0;

        tbl = getNumbers()
        
        for _, i in ipairs(tbl) do
            total = total + i;
        end

        return total;
    ";

    Script script = new Script();

    script.Globals["getNumbers"] = (Func<Script, Table>)(GetNumberTable);

    DynValue res = script.DoString(scriptCode);

    return res.Number;
}

{% endhighlight %}

You can see how easy is to operate with tables using the ``Table`` object. 

There are two things to notice however:

* To create a new ``Table`` object you must have a reference to the executing script
* If you have a ``Script`` parameter in a CLR function which is available to the Lua script, MoonSharp will fill it for you. This also happens with the (less likely to be used this way) ``ScriptExecutionContext`` and ``CallbackArguments`` types. Don't worry if you don't know what these do, they aren't needed to get the basics of MoonSharp working!

> As a good practice, always keep the ``Script`` object around, if you can. 
> There are some things (like creating tables) which can only be done using the ``Script`` object.


#### Accepting a table

A table is converted automatically to a ``List<T>``. For example:

{% highlight csharp %}
public static double TableTestReverse()
{
	string scriptCode = @"    
		return dosum { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 }
	";

	Script script = new Script();

	script.Globals["dosum"] = (Func<List<int>, int>)(l => l.Sum());

	DynValue res = script.DoString(scriptCode);

	return res.Number;
}
{% endhighlight %}


However this can potentially create problems on some platforms (read: iOS). 
There are a ton of ways around this problem (which you'll see in other tutorials) but suffice to say that the following
works with no issues and is faster to boot:


{% highlight csharp %}
public static double TableTestReverseSafer()
{
	string scriptCode = @"    
		return dosum { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 }
	";

	Script script = new Script();

	script.Globals["dosum"] = (Func<List<object>, int>)(l => l.OfType<int>().Sum());

	DynValue res = script.DoString(scriptCode);

	return res.Number;
}
{% endhighlight %}

Another way around, even faster, could have been using a ``Table`` object:

{% highlight csharp %}
static double Sum(Table t)
{
    var nums = from v in t.Values
               where v.Type == DataType.Number
               select v.Number;

    return nums.Sum();
}


private static double TableTestReverseWithTable()
{
    string scriptCode = @"    
        return dosum { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 }
    ";

    Script script = new Script();

    script.Globals["dosum"] = (Func<Table, double>)Sum;

    DynValue res = script.DoString(scriptCode);

    return res.Number;
}
{% endhighlight %}

But here we have to deal with ``DynValue``(s).

To understand all of this, we need to dig a little deeper on how MoonSharp maps Lua types to C# types and viceversa.. in the next part.



































