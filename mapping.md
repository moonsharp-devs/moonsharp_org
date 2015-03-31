---
layout: tutorial
title: Auto-conversions explained
subtitle: .. no code this time.
---

Before we dig deeper in MoonSharp and CLR integration, we need to clarify how types are mapped back and forth.
Sadly, back is very different from forth and so we will analyze the two separately.


<div class="alert alert-info" role="alert">
Isn't this a bit too complex? - you might ask.

Sure it is. Automatic things are nice and good but when they fail, they fail in terribly complex ways.
When in doubt or the thing gets too complicated, just use DynValue(s) instead. Not only you would gain in sanity and simplicity, it's also faster!
Or, use custom converters.
</div>


#### Custom converters

It's possible to customize the conversion process, however the setting is global and affects all scripts.
To customize conversions, simply set the appropriate callbacks to Script.GlobalOptions.CustomConverters.

For example, if we want all StringBuilders to be converted to uppercase strings when conversion from CLR to script happens, do:

{% highlight csharp %}

Script.GlobalOptions.CustomConverters.SetClrToScriptCustomConversion<StringBuilder>(
	v => DynValue.NewString(v.ToString().ToUpper()));

{% endhighlight %}


If we want to customize how all tables are converted when they should match a IList<int> we can write

{% highlight csharp %}

Script.GlobalOptions.CustomConverters.SetScriptToClrCustomConversion(DataType.Table, typeof(IList<int>),
	v => new List<int>() { ... });

{% endhighlight %}

If a converter returns *null*, the system will behave as if no custom converter existed.



#### Auto-Conversion of CLR types to MoonSharp types

This conversion is applied in the following situations:

* When returning an object from a function called by script
* When returning an object from a property in a user data
* When setting a value in a table using the indexing operator
* When calling DynValue.FromObject
* When using any overload of functions which takes a System.Object in place of a DynValue


This conversion is actually quite simple. The following table explains the conversions:

<div class="table-responsive">
<table class="table table-striped table-condensed">
<tr><th>CLR type</th><th>C# friendly name</th><th>Lua type</th><th>Notes</th></tr>
<tr><td>void</td><td></td><td>(no value)</td><td>This can be applied to return values of methods only.</td></tr>
<tr><td>null</td><td></td><td>nil</td><td>Any null will be converted to nil.</td></tr>
<tr><td>MoonSharp.Interpreter.DynValue</td><td></td><td>*</td><td>The DynValue is passed through.</td></tr>
<tr><td>System.SByte</td><td>sbyte</td><td>number</td><td></td></tr>
<tr><td>System.Byte</td><td>byte</td><td>number</td><td></td></tr>
<tr><td>System.Int16</td><td>short</td><td>number</td><td></td></tr>
<tr><td>System.UInt16</td><td>ushort</td><td>number</td><td></td></tr>
<tr><td>System.Int32</td><td>int</td><td>number</td><td></td></tr>
<tr><td>System.UInt32</td><td>uint</td><td>number</td><td></td></tr>
<tr><td>System.Int64</td><td>long</td><td>number</td><td>The conversion can lead to a silent precision loss.</td></tr>
<tr><td>System.UInt64</td><td>ulong</td><td>number</td><td>The conversion can lead to a silent precision loss.</td></tr>
<tr><td>System.Single</td><td>float</td><td>number</td><td></td></tr>
<tr><td>System.Decimal</td><td>decimal</td><td>number</td><td>The conversion can lead to a silent precision loss.</td></tr>
<tr><td>System.Double</td><td>double</td><td>number</td><td></td></tr>
<tr><td>System.Boolean</td><td>bool</td><td>boolean</td><td></td></tr>
<tr><td>System.String</td><td>string</td><td>string</td><td></td></tr>
<tr><td>System.Text.StringBuilder</td><td></td><td>string</td><td></td></tr>
<tr><td>System.Char</td><td>char</td><td>string</td><td></td></tr>
<tr><td>MoonSharp.Interpreter.Table</td><td></td><td>table</td><td></td></tr>
<tr><td>MoonSharp.Interpreter.CallbackFunction</td><td></td><td>function</td><td></td></tr>
<tr><td>System.Delegate</td><td></td><td>function</td><td></td></tr>
<tr><td>System.Object</td><td>object</td><td>userdata</td><td>Only if the type has been registered for userdata.</td></tr>
<tr><td>System.Type</td><td></td><td>userdata</td><td>Only if the type has been registered for userdata, static members access.</td></tr>
<tr><td>MoonSharp.Interpreter.Closure</td><td></td><td>function</td><td></td></tr>
<tr><td>System.Reflection.MethodInfo</td><td></td><td>function</td><td></td></tr>
<tr><td>System.Collections.IList</td><td></td><td>table</td><td>The resulting table will be indexed 1-based. All values are converted using these rules. </td></tr>
<tr><td>System.Collections.IDictionary</td><td></td><td>table</td><td>All keys and values are converted using these rules.</td></tr>
<tr><td>System.Collections.IEnumerable</td><td></td><td>iterator</td><td>All values are converted using these rules.</td></tr>
<tr><td>System.Collections.IEnumerator</td><td></td><td>iterator</td><td>All values are converted using these rules.</td></tr>
</table>
</div>

This includes a pretty good coverage of collections, as most collections do implement IList, IDictionary, IEnumerable and/or IEnumerator.
In case you are writing your own collections, remember to implement one of these non-generic interfaces.

Every value which cannot be converted using this logic will throw a ScriptRuntimeException.


#### Default Auto-Conversion of MoonSharp types to CLR types

The opposite conversion is quite more complex. In fact, there exist two different conversion paths - the "default" one, and the "constrained" one. The first applies everytime you ask to convert a DynValue to an object without specifying what actually you want to receive, the other when there is a target Type to match.

This is used:

* When calling the DynValue.ToObject 
* When retrieving values from a table using indexers
* In some specific subcase of the constrained conversion (see below)

Here we see the default conversion. It's actually simple:

<div class="table-responsive">
<table class="table table-striped table-condensed">
<tr><th>MoonSharp type</th><th>CLR type</th><th>Notes</th></tr>
<tr><td>nil</td><td>null</td><td>Applied to every value which is nil.</td></tr>
<tr><td>boolean</td><td>System.Boolean</td><td></td></tr>
<tr><td>number</td><td>System.Double</td><td></td></tr>
<tr><td>string</td><td>System.String</td><td></td></tr>
<tr><td>function</td><td>MoonSharp.Interpreter.Closure</td><td>If DataType is Function.</td></tr>
<tr><td>function</td><td>MoonSharp.Interpreter.CallbackFunction</td><td>If DataType is ClrFunction.</td></tr>
<tr><td>table</td><td>MoonSharp.Interpreter.Table</td><td></td></tr>
<tr><td>tuple</td><td>MoonSharp.Interpreter.DynValue[]</td><td></td></tr>
<tr><td>userdata</td><td>(special)</td><td>Returns the object stored in userdata. If a "static" userdata, the Type is returned.</td></tr>
</table>
</div>

Every value which cannot be converted using this logic will throw a ScriptRuntimeException.

#### Constrained Auto-Conversion of MoonSharp types to CLR types

The constrained auto-conversion is a lot more complex though. In this case a MoonSharp value must be stored in a CLR variable of a given Type and there are pretty much infinite ways to do these conversions. 

This is used:

* When calling DynValue.ToObject&lt;T&gt;
* When converting a script value to a parameter in a CLR function call, or property set

MoonSharp attempts very hard to convert values, but the conversion surely shows some limits, specially when tables are involved.

In this case, the conversion is more a process than a simple table of mapping, so let's analyze the target types one by one.

##### Special cases

* if the target is of type MoonSharp.Interpreter.DynValue it is not converted and the original value is returned.
* If the target is of type System.Object, the default conversion, detailed before, is applied.
* In case of a nil value to be mapped, null is mapped to reference types and nullable value types and an attempt to match a default value is used in some cases (for example function calls for which a default is specified) for non-nullable value types, otherwise an exception is thrown.
* In case of no value provided, the default value is used if possible.

##### Strings

Strings can be automatically converted to System.String, System.Text.StringBuilder or System.Char.

##### Booleans

Booleans can be automatically converted to System.Boolean and/or System.Nullable&lt;System.Boolean&gt;. They can also be converted to System.String, System.Text.StringBuilder or System.Char.

##### Numbers 

Numbers can be automatically converted over System.SByte, System.Byte, System.Int16, System.UInt16, System.Int32, System.UInt32, System.Int64, System.UInt64, System.Single, System.Decimal, System.Double and their nullable counterparts. They can also be converted to System.String, System.Text.StringBuilder or System.Char.

##### Functions

Functions are converted to MoonSharp.Interpreter.Closure and MoonSharp.Interpreter.ClrFunction depending on their subtype. No better conversion exists.

##### Userdata

Userdatas are converted only if they are not "static" (see the userdata section in the tutorials).  They are converted if the desired type can be assigned with the object to be converted.

They can also be converted to System.String, System.Text.StringBuilder or System.Char, by calling the object ToString() method.

##### Tables

Tables can be converted to:

* MoonSharp.Interpreter.Table - of course
* Types assignable from Dictionary&lt;DynValue, DynValue&gt;.
* Types assignable from Dictionary&lt;object, object&gt;. Keys and values are mapped using the default mapping.
* Types assignable from List&lt;DynValue&gt;.
* Types assignable from List&lt;object&gt;. Elements are mapped using the default mapping.
* Types assignable from DynValue[].
* Types assignable from object[]. Elements are mapped using the default mapping.
* T[], IList<T>, List<T>, ICollection<T>, IEnumerable<T>, where T is a convertible type (including other lists, etc.) 
* IDictionary<K,V>, Dictionary<K,V>, where K and V are a convertible type (including other lists, etc.)

Note that conversion to generics and typed arrays have the following limitations:

* They are slower, as types are created at runtime through reflection
* If the item type is a value type, there is the potential that it will introduce incompatibilities with iOS and other platforms running in AOT mode. Do tests beforehand.
* To compensate with these problems, you can always add custom converters in a later stage of development!


<div class="alert alert-info" role="alert">
Some conversions (for example to List<int>) might cause problems on AOT platforms like Unity/iOS. 
Register a custom converter to workaround issues of this kind.
</div>


#### ByRef parameters (ref/out in C#)

ByRef method parameters are correctly marshalled by MoonSharp, as multiple return values.
This support is not without side effects, as methods with ByRef parameters cannot be optimized.

Let's say we have this C# method (exposed in a myobj userdata for the sake of the argument)

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


<div class="alert alert-info" role="alert">
While supported, ByRef params causes the method to always be invoked using reflection, thus potentially slowing
down performance on non-AOT platforms.
</div>


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
For example these are valid lines of code, given that "o" is an instance of the above class:

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
scenarios going through metamethods, but if the __index field of the metatable is set to a userdata (also, recursively),
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



#### Operators

 





