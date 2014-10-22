---
layout: tutorial
title: Sharing objects
subtitle: .. and private properties.
---

A handy feature of MoonSharp is the ability to share a .NET object with a script.
The object must follow some little charateristics before being able to be shared:
	
* All its public methods must not have overloads (default values are accepted). 
* The type must be registered beforehand

If these conditions are satisfied, all public properties and methods are accessible from script.

It is recommended to use dedicated objects as an interface between CLR code and script code (as opposed to exposing the application internal model
to the script). A number of design patterns (Adapter, Facade, Proxy, etc.) may help in designing such an interface layer. This is particularly important to:

* Limit what scripts can and cannot do
* Offer an interface which is meaningful for the script authors
* Document the interface separately
* Allow the internal logic and model to change without breaking scripts


#### Keep it simple

Ok, let's go with the first example.

 
{% highlight csharp %} 

[MoonSharpUserData]
class MyClass
{
	public double calcHypotenuse(double a, double b)
	{
		return Math.Sqrt(a * a + b * b);
	}
}

double CallMyClass1()
{
	string scriptCode = @"    
		return obj.calcHypotenuse(3, 4);
	";

	// Automatically register all MoonSharpUserData types
	UserData.RegisterAssembly();

	Script script = new Script();

	// Pass an instance of MyClass to the script in a global
	script.Globals["obj"] = new MyClass();

	DynValue res = script.DoString(scriptCode);

	return res.Number;
}

{% endhighlight %}

Here we:

* Defined a class with [MoonSharpUserData] attribute
* Passed an instance of a MyClass object as a global in the script
* Invoked a MyClass method from a script. All the mapping rules for callbacks apply

#### A little more complex

Let's try a little more complex example.

{% highlight csharp %}

class MyClass
{
	public double calcHypotenuse(double a, double b)
	{
		return Math.Sqrt(a * a + b * b);
	}
}

static double MyClass2()
{
	string scriptCode = @"    
		return obj.calcHypotenuse(3, 4);
	";

	// Register just MyClass, explicitely.
	UserData.RegisterType<MyClass>();

	Script script = new Script();

	// create a userdata, again, explicitely.
	DynValue obj = UserData.Create(new MyClass());
	
	script.Globals.Set("obj", obj);

	DynValue res = script.DoString(scriptCode);

	return res.Number;
}

{% endhighlight %}

The big differences here are:

* No [MoonSharpUserData] attribute. We don't need it anymore.
* Instead of RegisterAssembly, we call RegisterType to register a specific type.
* We create the userdata DynValue explicitely.

#### Calling static methods and properties

Let's say our class has the calcHypotenuse method static.

{% highlight csharp %}

[MoonSharpUserData]
class MyClassStatic
{
	public static double calcHypotenuse(double a, double b)
	{
		return Math.Sqrt(a * a + b * b);
	}
}

{% endhighlight %}

We can call it in two ways.

First way - Static methods can be called from an instance transparently - no need to do anything, everything is automatic


{% highlight csharp %}

double MyClassStaticThroughInstance()
{
	string scriptCode = @"    
		return obj.calcHypotenuse(3, 4);
	";

	// Automatically register all MoonSharpUserData types
	UserData.RegisterAssembly();

	Script script = new Script();

	script.Globals["obj"] = new MyClassStatic();

	DynValue res = script.DoString(scriptCode);

	return res.Number;
}

{% endhighlight %}


Alternative way - A placeholder userdata can be created, by directly passing the type (or using UserData.CreateStatic method) :


{% highlight csharp %}

double MyClassStaticThroughPlaceholder()
{
	string scriptCode = @"    
		return obj.calcHypotenuse(3, 4);
	";

	// Automatically register all MoonSharpUserData types
	UserData.RegisterAssembly();

	Script script = new Script();

	script.Globals["obj"] = typeof(MyClassStatic);

	DynValue res = script.DoString(scriptCode);

	return res.Number;
}

{% endhighlight %}


#### Should ':' or '.' be used ?

A good question is, should (given the code in the above examples) this syntax be used 

{% highlight lua %}

return obj.calcHypotenuse(3, 4);

{% endhighlight %}

or this ?

{% highlight lua %}

return obj:calcHypotenuse(3, 4);

{% endhighlight %}

99.999% of the time, it makes no difference. MoonSharp knows that a call is being done on a userdata and behaves accordingly.

There are corner cases where it might make a difference - for example if a property returns a delegate and you are going to call that delegate immediately,
with the original object as an instance. It's a remote scenario, and you have to handle it manually when that happens.

