---
layout: tutorial
title: Proxy objects
subtitle: How to encapsulate your implementation and yet offer a meaningful object model to scripts
---

In a real world scenario, scripts will often be outside your control. These brings several issues to the table, among which:

* Security: this part is off topic for this specific page, but please read the section about sandboxing if your scripts will not come from a strictly controlled environment.

* Backward and forward compatibility: MoonSharp does its best to avoid introducing compatibility problems with the past or in the future, but your API design is your responsibility!


One key method to achieve both the above is to encapsulate your implementation details so that no fields which APIs aren't supposed to call are, in fact, called and that you can evolve your internal model as you please, with little worries of accidentally breaking scripts.

There are two ways to encapsulate, one is to replicate all your APIs in some really complex ways. The other is through "proxy objects" (a sort of "DTOs for scripts").

The concept is very very simple. For every type exposed to script that you want to encapsulate, you must provide a "proxy type", which is a class, which operates on an instance of the encapsulated (target) type.

An example is worth a thousand words:

{% highlight csharp %} 

// Declare a proxy class
class MyProxy
{
	MyType target;

	[MoonSharpHidden]
	public MyProxy(MyType p)
	{
		this.target = p;
	}

	public int GetValue() { return target.GetValue(); }
}

// Register the proxy, using a function which creates a proxy from the target type
UserData.RegisterProxyType<MyProxy, MyType>(r => new MyProxy(r));

// Test with a script - only the proxy type is really exposed to scripts, yet everything it works
// just as if the target type was actually used..

Script S = new Script();

S.Globals["mytarget"] = new MyType(...);
S.Globals["func"] = (Action<MyType>)SomeFunction;

S.DoString(@"
		x = mytarget.GetValue(); 
		func(mytarget); 
");

{% endhighlight %}


> Some clever tricks can be done with proxy objects, apart from encapsulation.
> 
> One is very simple, yet very useful - provide correct access to value types; for example you can wrap Unity ``Transform`` class - which is entirely made of value types, but is a reference type - with a different interface which preserves the references correctly!






