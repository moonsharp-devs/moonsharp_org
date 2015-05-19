---
layout: tutorial
title: Sharing objects
subtitle: Let Lua and C# speak each other.
---

> Note : Some features listed in this page reflect the current status of the master branch (so, they might be features missing from the latest release).

A handy feature of MoonSharp is the ability to share a .NET object with a script.

A type by default will share all its public methods, public properties, public events and public fields with Lua scripts. The MoonSharpVisible attribute can be used to override this default visibility. 

It is recommended to use dedicated objects as an interface between CLR code and script code (as opposed to exposing the application internal model
to the script). A number of design patterns (Adapter, Facade, Proxy, etc.) may help in designing such an interface layer. This is particularly important to:

* Limit what scripts can and cannot do (security! do you want your mod authors to find a way to delete end user personal files ?)
* Offer an interface which is meaningful for the script authors
* Document the interface separately
* Allow the internal logic and model to change without breaking scripts

For these reasons MoonSharp by default requires an explicit registration of the types which will be available to scripts.

If you are in a scenario where scripts can be trusted, you can globally enable autoregistration with ``UserData.RegistrationPolicy = InteropRegistrationPolicy.Automatic;``. It's dangerous, you have been warned.


So, let's see what we have on the menu:

* [Let's have a word about Type descriptors first](#theory)] - a little theory explaining what happens behind the scenes and how you can override the whole interop system
* [Keep it simple](#simple) - the simplest way to get started
* [A little more complex](#simple2) - we dig in, adding a little more complexity and details
* [Calling static members](#statics) - how to call statics
* [Should ':' or '.' be used ?](#colon) - simple question about how to call methods
* [Overloads](#overload) - how overloads are handled
* [ByRef parameters (ref/out in C#)](#byref) - how ref/out params are handled
* [Indexers](#indexers) - how to handle indexers
* [Operators and metamethods on userdata](#operators) - how to overload operators, etc.
* [Extension Methods](#extmethods) - how to use extension methods
* [Events](#events) - how to use events
* [Interop Access Modes](#interopaccessmode) - what the interop access mode is and how it works
* [Changing visibility with MoonSharpVisible](#moonsharpvisible) - how to override the visibility of members


A lot, so let's start.

<a name="theory"></a>

#### Let's have a word about Type descriptors first

First some little theory of how interop is implemented. Every CLR type is wrapped into a "type descriptor" which has the role of describing the CLR type to scripts. Registering a ``Type`` for 
interop means associating the ``Type`` with a descriptor (which MoonSharp can create itself) which will be used to dispatch methods, properties, etc.

From the next section on, we will refer to the "automatic" descriptors MoonSharp offers, but you can implement your own descriptors for speed, added funcionality, security, etc.

If you want to implement your own descriptors (**which is not easy and should not be done unless you need to**) you can follow these paths:

* Create an ad-hoc <a href="http://www.moonsharp.org/reference/html/fef3382e-b174-9b7a-72c3-8fec1b0a5fab.htm">``IUserDataDescriptor``</a> to describe your own type(s) - this is the hardest way
* Have your type implement the <a href="http://www.moonsharp.org/reference/html/c2467704-ea00-90b6-45f3-1a23e69a27ff.htm">``IUserDataType``</a> interface. This is easier, but means you cannot handle static members without an object instance.
* Extend or embed <a href="http://www.moonsharp.org/reference/html/8d492fd5-3190-822a-2315-6a43ee87d6dd.htm">``StandardUserDataDescriptor``</a> and change the aspects you need while keeping the rest of the behaviour. 

In order to help the creation of descriptors, the following classes are available:

* <a href="http://www.moonsharp.org/reference/html/8d492fd5-3190-822a-2315-6a43ee87d6dd.htm">``StandardUserDataDescriptor``</a> - this is the type descriptor MoonSharp implements
* <a href="http://www.moonsharp.org/reference/html/5851ad10-9bdc-63b8-f87e-32b2fe8f5a90.htm">``StandardUserDataMethodDescriptor``</a> - this is a descriptor for a single method/function
* <a href="http://www.moonsharp.org/reference/html/6f70669b-d15d-3317-6c6c-6622a5229dd7.htm">``StandardUserDataOverloadedMethodDescriptor``</a> - this is a descriptor for overloaded and/or extended methods
* <a href="http://www.moonsharp.org/reference/html/b950e338-488b-6e4c-d6eb-bebcecc38541.htm">``StandardUserDataPropertyDescriptor``</a> - this is a descriptor for a single property
* <a href="http://www.moonsharp.org/reference/html/a29391b0-a33d-935f-8809-6d7c3ea0f07e.htm">``StandardUserDataFieldDescriptor``</a> - this is a descriptor for a single field

> A little note about interop with value types as userdata. 
> 
> Just as if calling a function passing a value type as a parameter, the script would operate on a copy of the userdata, so, for example, changing a field in the userdata would not reflect on the original value. Again, this is not any different from standard behavior of value types, but it's enough to catch people by surprise.
>
> Additionally, value types do not support the whole spectrum of optimizations as reference types do, so some operations might be slower on value types than reference types.



<a name="simple"></a>

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

* Defined a class with <a href="http://www.moonsharp.org/reference/html/b9ab19ce-fa22-1cd3-e3c7-53b2b63bb41a.htm">``[MoonSharpUserData]``</a> attribute
* Passed an instance of a ``MyClass`` object as a global in the script
* Invoked a ``MyClass`` method from a script. All the mapping rules for callbacks apply

<a name="simple2"></a>

#### A little more complex

Let's try a little more complex example.

{% highlight csharp %}

class MyClass
{
	public double CalcHypotenuse(double a, double b)
	{
		return Math.Sqrt(a * a + b * b);
	}
}

static double CallMyClass2()
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

* No ``[MoonSharpUserData]`` attribute. We don't need it anymore.
* Instead of ``RegisterAssembly``, we call ``RegisterType`` to register a specific type.
* We create the userdata ``DynValue`` explicitely.

Also, note how the method was called ``CalcHypotenuse`` in C# code, but is called as ``calcHypotenuse`` by Lua scripts.

As long as the other versions do not exist, MoonSharp automatically adjusts the case in some limited ways to match members, for better consistency between different languages syntax conventions.
For example, a member called ``SomeMethodWithLongName`` can be accessed from a lua script also as ``someMethodWithLongName`` or ``some_method_with_long_name``.



<a name="statics"></a>

#### Calling static members

Let's say our class has the ``calcHypotenuse`` method static.

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


Alternative way - A placeholder userdata can be created, by directly passing the type (or using ``UserData.CreateStatic`` method) :


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


<a name="colon"></a>

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


<a name="overload"></a>

#### Overloads

Overloaded methods are supported. The dispatch of overloaded method is somewhat a dark magic and is not as deterministic as C# overload dispatch is.
This is due to the fact that some ambiguities exist. For example, an object can declare these two methods:

{% highlight csharp %}

void DoSomething(int i) { ... }
void DoSomething(float f) { ... }

{% endhighlight %}

How can MoonSharp know which method to dispatch to, given that all numbers in Lua are *double* ? 

To solve this issue, MoonSharp calculates an heuristic factor for all overloads given the input types and chooses the best overload.
If you think MoonSharp is resolving an overload in a wrong way, please report to the forums, for the heuristic to be calibrated.

> MoonSharp tries as much to be stable with the heuristic weights, and in case of a draw of scores between methods, it always deterministically choses the same one
> (to provide a consistent experience among builds and platforms).
>
> This said, it's entirely possible that MoonSharp picks an overload which is different than the one you think of. It's extremely important then that overloads perform
> equivalent jobs so that the impact of calling the wrong overload is minimized. This should be a best practice anyway, but it's worth reinforcing the concept here.


<a name="byref"></a>

#### ByRef parameters (ref/out in C#)

``ByRef`` method parameters are correctly marshalled by MoonSharp, as multiple return values.
This support is not without side effects, as methods with ``ByRef`` parameters cannot be optimized.

Let's say we have this C# method (exposed in a ``myobj`` userdata for the sake of the argument)

{% highlight csharp %}

public string ManipulateString(string input, ref string tobeconcat, out string lowercase)
{
	tobeconcat = input + tobeconcat;
	lowercase = input.ToLower();
	return input.ToUpper();
}

{% endhighlight %}

We can call (and get the results) the method from Lua code in this way:

{% highlight lua %}

x, y, z = myobj:manipulateString('CiAo', 'hello');

-- x will be "CIAO"
-- y will be "CiAohello"
-- z will be "ciao"

{% endhighlight %}


> While supported, ByRef params causes the method to always be invoked using reflection, thus potentially slowing
> down performance on non-AOT platforms (AOT platforms are already slow.. send your complaints to Apple, not me).


<a name="indexers"></a>

#### Indexers 

C# allows indexer methods to be created. For example:

{% highlight csharp %}

class IndexerTestClass
{
	Dictionary<int, int> mymap = new Dictionary<int, int>();

	public int this[int idx]
	{
		get { return mymap[idx]; }
		set { mymap[idx] = value; }
	}

	public int this[int idx1, int idx2, int idx3]
	{
		get { int idx = (idx1 + idx2) * idx3; return mymap[idx]; }
		set { int idx = (idx1 + idx2) * idx3; mymap[idx] = value; }
	}
}

{% endhighlight %}

As an extension to the Lua language, MoonSharp allows an expression list inside brackets to index userdata.
For example these are valid lines of code, given that ``o`` is an instance of the above class:

{% highlight lua %}

-- sets the value of an indexer
o[5] = 19; 		

-- use the value of an indexer
x = 5 + o[5]; 	

-- sets the value of an indexer using multiple indices (not standard Lua!)
o[1,2,3] = 19; 		

-- use the value of an indexer using multiple indices (not standard Lua!)
x = 5 + o[1,2,3]; 	

{% endhighlight %}

Note that using multiple indices on anything which is not a userdata will raise an error. This includes
scenarios going through metamethods, but if the ``__index`` field of the metatable is set to a userdata (also, recursively),
multi-indexing is supported.

In short, this works:

{% highlight lua %}

m = { 
	__index = o,
	__newindex = o
}

t = { }

setmetatable(t, m);

t[10,11,12] = 1234; return t[10,11,12];";

{% endhighlight %}

and this does not:

{% highlight lua %}

m = { 
	-- we can't even write meaningful functions here, but let's pretend...
	__index = function(obj, idx) return o[idx] end,     
	__newindex = function(obj, idx, val) end
}

t = { }

setmetatable(t, m);

t[10,11,12] = 1234; return t[10,11,12];";

{% endhighlight %}





<a name="operators"></a>

#### Operators and metamethods on userdata

Overloaded operators are supported. 

Following is the description on how the standard descriptor dispatches operators, but you can 
see working examples <a href="https://github.com/xanathar/moonsharp/blob/master/src/MoonSharp.Interpreter.Tests/EndToEnd/UserDataMetaTests.cs"> in this unit test code. </a>





##### Explicit metamethod implementation 

First, if one or more static methods decorated with *MoonSharpUserDataMetamethod* are implemented, 
these are used to dispatch the corresponding metamethod. Note that these methods exist, they will take over
any other of the following criteria.

``__pow``, ``__concat``, ``__call``, ``__pairs`` and ``__ipairs`` can only be implemented this way (short of using a custom descriptor).

For example these will implement the concat (``..``) operator:

{% highlight csharp %}
[MoonSharpUserDataMetamethod("__concat")]
public static int Concat(ArithmOperatorsTestClass o, int v)
{
	return o.Value + v;
}

[MoonSharpUserDataMetamethod("__concat")]
public static int Concat(int v, ArithmOperatorsTestClass o)
{
	return o.Value + v;
}

[MoonSharpUserDataMetamethod("__concat")]
public static int Concat(ArithmOperatorsTestClass o1, ArithmOperatorsTestClass o2)
{
	return o1.Value + o2.Value;
}
{% endhighlight %}



##### Implicit metamethod implementation for arithmetic operators

Arithmetic operators are automatically handled by operator overloads if found.

{% highlight csharp %}
public static int operator +(ArithmOperatorsTestClass o, int v)
{
	return o.Value + v;
}

public static int operator +(int v, ArithmOperatorsTestClass o)
{
	return o.Value + v;
}

public static int operator +(ArithmOperatorsTestClass o1, ArithmOperatorsTestClass o2)
{
	return o1.Value + o2.Value;
}
{% endhighlight %}

it will be possible to use the operator ``+`` between numbers and this object in Lua scripts.

The addition, subtraction, multiplication, division, modulus, and unary negation operators are supported this way.


##### Comparison operators, length operator and __iterator metamethod

Equality operators (``==`` and ``~=``) are automatically resolved using ``System.Object.Equals``. 

Comparison operators (``<``, ``>=``, etc.) are automatically resolved using ``IComparable.CompareTo``, if the object implements ``IComparable``.

The length (``#``) operator is dispatched to the ``Length`` or ``Count`` properties if the object implements those properties.

Finally the ``__iterator`` metamethod is automatically dispatched to ``GetEnumerator``, if the class implements ``System.Collections.IEnumerable`` .


<a name="extmethods"></a>

#### Extension Methods

Extension methods are supported.

Extension methods must be registered with <a href="http://www.moonsharp.org/reference/html/c4d99b23-3d7f-4448-d942-cc19518a7633.htm">``UserData.RegisterExtensionType``</a> or through a <a href="http://www.moonsharp.org/reference/html/67401cf0-ad24-9e4e-5336-fe2d35220b9a.htm">``RegisterAssembly(<assembly>, true)``</a> .
The first will register a single type containing extension methods, the second registers all extension types contained in the specified assembly.

Extension methods are resolved along with other overloads on the methods.




<a name="events"></a>

#### Events

Events are also supported, but in a rather minimalistic way.
Only events matching these constraints are supported:

* The event must be declared in a reference type
* The event must implement both add and remove methods
* The event handler must have a return type of System.Void (in VB.NET, must be a Sub)
* The event handler must have 16 parameters or less
* The event handler must not have value type parameters or by-ref parameters
* The event handler signature must not contain pointers or unresolved generics
* All the parameters of the event handler must be convertible to MoonSharp types

These constraints are present to avoid building code at runtime as much as possible.

While they might seem limiting, for the most part they actually reflect some best practices in the design of events; they are more than enough
to support event handlers of ``EventHandler`` and ``EventHandler<T>`` types, which are by far the most common ones (provided at least ``EventArgs`` is registered as a user data). 


Here is a simple example using an event:

{% highlight csharp %}
class MyClass
{
	public event EventHandler SomethingHappened;

	public void RaiseTheEvent()
	{
		if (SomethingHappened != null)
			SomethingHappened(this, EventArgs.Empty);
	}
}

static void Events()
{
	string scriptCode = @"    
		function handler(o, a)
			print('handled!', o, a);
		end

		myobj.somethingHappened.add(handler);
		myobj.raiseTheEvent();
		myobj.somethingHappened.remove(handler);
		myobj.raiseTheEvent();
	";

	UserData.RegisterType<EventArgs>();
	UserData.RegisterType<MyClass>();
	Script script = new Script();
	script.Globals["myobj"] = new MyClass();
	script.DoString(scriptCode);
}
{% endhighlight %}

Note how the event is raised by Lua code this time, but it might be raised by C# as well, without any issue.

> Adding and removing event handlers are slow operations, being performed with reflection under a thread lock. On the other side, there aren't big performance penalties in handling events themselves.



<a name="interopaccessmode"></a>

#### A word on InteropAccessMode

If you typed all the examples so far in an IDE you might have noticed that most methods have an optional parameter of ``InteropAccessMode`` type.

An ``InteropAccessMode`` defines how the standard descriptors will handle callbacks to CLR things. The following values are available:

There is a ``UserData.DefaultAccessMode`` static property to specify which value is to be considered the default (currently, it's ``LazyOptimized``, unless changed).


<div class="table-responsive">
	<table class="table table-striped table-condensed">
		<tr>
			<th>Reflection</th>
			<td>Optimization is not performed and reflection is used everytime to access members. This is the slowest approach but saves a lot of memory if members are seldomly used.</td>
		</tr>
		<tr>
			<th>LazyOptimized</th>
			<td>This is a hint, and MoonSharp is free to "downgrade" this to <code>Reflection</code>. Optimization is done on the fly the first time a member is accessed. This saves memory for all members that are never accessed, at the cost of an increased script execution time.</td>
		</tr>
		<tr>
			<th>Preoptimized</th>
			<td>This is a hint, and MoonSharp is free to "downgrade" this to <code>Reflection</code>. Optimization is done in a background thread which starts at registration time. If a member is accessed before optimization is completed, reflection is used.</td>
		</tr>
		<tr>
			<th>BackgroundOptimized</th>
			<td>This is a hint, and MoonSharp is free to "downgrade" this to <code>Reflection</code>.  Optimization is done at registration time.</td>
		</tr>
		<tr>
			<th>HideMembers</th>
			<td>Members are simply not accessible at all. Can be useful if you need a userdata type whose members are hidden from scripts but can still be passed around to other functions. See also <code>AnonWrapper</code> and <code>AnonWrapper&lt;T&gt;</code>.</td>
		</tr>
		<tr>
			<th>Default</th>
			<td>Use the default access mode</td>
		</tr>
	</table>
</div>

> Note that many modes - specifically ``LazyOptimized``, ``Preoptimized`` and ``BackgroundOptimized`` - are just "hints" and MoonSharp is free to downgrade them to ``Reflection``. This happens, for example, in the case of platforms where code is compiled ahead of time, like the iPhone and the iPad.

<a name="moonsharpvisible"></a>

#### Changing visibility with MoonSharpVisible

It's possible to use the MoonSharpVisible attribute to override the default visibility of members. Here are some examples with comments - nothing hard:

{% highlight csharp %}
public class SampleClass
{
	// Not visible - it's private
	private void Method1() { }
	// Visible - it's public
	public void Method2() { }
	// Visible - it's private but forced visible by attribute
	[MoonSharpVisible(true)]
	private void Method3() { }
	// Not visible - it's public but forced hidden by attribute
	[MoonSharpVisible(false)]
	public void Method4() { }

	// Not visible - it's private
	private int Field1 = 0;
	// Visible - it's public
	public int Field2 = 0;
	// Visible - it's private but forced visible by attribute
	[MoonSharpVisible(true)]
	private int Field3 = 0;
	// Not visible - it's public but forced hidden by attribute
	[MoonSharpVisible(false)]
	public int Field4 = 0;

	// Not visible at all - it's private
	private int Property1 { get; set; }
	// Read/write - it's public
	public int Property2 { get; set; }
	// Readonly - it's public, but the setter is private
	public int Property3 { get; private set; }
	// Write only! - the MoonSharpVisible makes the getter hidden and the setter visible!
	public int Property4 { [MoonSharpVisible(false)] get; [MoonSharpVisible(true)] private set; }
	// Write only! - the MoonSharpVisible makes the whole property hidden but another attribute resets the setter as visible!
	[MoonSharpVisible(false)]
	public int Property5 { get; [MoonSharpVisible(true)] private set; }
	// Not visible at all - the MoonSharpVisible hides everything
	[MoonSharpVisible(false)]
	public int Property6 { get; set; }
	
	// Not visible - it's private
	private event EventHandler Event1;
	// Visible - it's public
	public event EventHandler Event2;
	// Visible - it's private but forced visible by attribute
	[MoonSharpVisible(true)]
	private event EventHandler Event3;
	// Not visible - it's public but forced hidden by attribute
	[MoonSharpVisible(false)]
	public event EventHandler Event4;
	// Not visible - visibility modifiers over add and remove are not currently supported!
	[MoonSharpVisible(false)]
	public event EventHandler Event5 { [MoonSharpVisible(true)] add { } [MoonSharpVisible(true)] remove { } }	

}
{% endhighlight %}

