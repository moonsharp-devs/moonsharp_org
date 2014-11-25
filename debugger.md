---
layout: tutorial
title: Debugger integration
subtitle: How to offer a debugger to your users. Or yourself.
---

There are several ways to integrate a debugger with MoonSharp applications, ranging from the very simple to quite complex, depending on how much
you want to customize the debugging experience.








This procedure will just give you a taste of MoonSharp and its simplicity and power.
There are better ways to use MoonSharp, but this is the easiest one.

Most tutorials on this site are C# only - this is the only page with some pointers to get VB.NET users started.

<div class="alert alert-success" role="alert">
MoonSharp is compatible with all CLR languages - C#, VB.NET, C++/CLI, F#, Boo and whatever else you like. 
It could also work with DLR languages (IronPython, IronRuby, etc.).

Examples, however, will only be produced for C# (and in the case of this introductory page, VB.NET) as 
keeping examples in all languages is a huge effort.

</div>






#### Step 1: getting MoonSharp in your IDE

The first step is to get MoonSharp in your IDE.
There are several paths to this, depending on what IDE you are using - Visual Studio, MonoDevelop, SharpDevelop or Unity.

##### In Visual Studio, with Nuget

In the package manager, type:

{% highlight PowerShell %}

PM> Install-Package MoonSharp 

{% endhighlight %}

Otherwise, right click on "References", "Manage NuGet Packages", open the "Online" dropdown, select nuget.org and search for a 
package named "MoonSharp".


##### In Visual Studio and other IDEs

Refer to the documentation of your IDE to add **MoonSharp.Interpreter.dll** and **Antlr4.Runtime.net35.dll**, contained in the "library" folder
of the MoonSharp distribution, as dependencies.


##### In Unity

Put the **MoonSharp.Interpreter.dll** and **Antlr4.Runtime.net35.dll**, contained in the "library" folder
of the MoonSharp distribution, in your Assets folder.

After this, add them as references from MonoDevelop-Unity.



#### Step 2: Importing the namespace
Import the **MoonSharp.Interpreter** namespace into your code, adding the following lines at the top of your code:

In C#:
{% highlight csharp %}
 
using MoonSharp.Interpreter;

{% endhighlight %}


In VB.NET:
{% highlight vbnet %}
 
Imports MoonSharp.Interpreter

{% endhighlight %}

#### Step 3: Call into a script 
Here we create a function **MoonSharpFactorial** which calculates a factorial using MoonSharp.

In C#:

{% highlight csharp %}

double MoonSharpFactorial()
{
	string script = @"    
		-- defines a factorial function
		function fact (n)
			if (n == 0) then
				return 1
			else
				return n*fact(n - 1)
			end
		end

		return fact(5)";

	DynValue res = Script.RunString(script);
	return res.Number;
}

{% endhighlight %}

In VB.NET:

{% highlight vbnet %}

Function MoonSharpFactorial() As Double
	Dim scriptCode As String = "-- defines a factorial function" & vbCrLf &
			"function fact (n)" & vbCrLf & _
				"if (n == 0) then" & vbCrLf & _
					"return 1" & vbCrLf & _
				"else" & vbCrLf & _
					"return n*fact(n - 1)" & vbCrLf & _
				"end" & vbCrLf & _
			"end" & vbCrLf & _
			"return fact(5)" & vbCrLf

	Dim res As DynValue = Script.RunString(scriptCode)
	Return res.Number
End Function

{% endhighlight %}

#### Step 4: Profit!

To run your code, of course, you just have to call the MoonSharpFactorial function 
somewhere else in your code.






        
		
		
		


