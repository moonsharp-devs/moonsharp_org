---
layout: tutorial
title: Getting Started
subtitle: A quick guide to your first MoonSharp project
---

This walkthrough will just give you a taste of MoonSharp and its simplicity and power.
There are better ways to use MoonSharp, but this is the easiest one. Other tutorials will dig further and teach you how to use MoonSharp at its best.
For now, let's get started.


Most tutorials on this site are C# only - this is the only page with some pointers to get VB.NET users started.



> MoonSharp is compatible with all CLR languages - C#, VB.NET, C++/CLI, F#, Boo and whatever else you like. It could also work with DLR languages (IronPython, IronRuby, etc.).
> 
> Examples, however, will only be produced for C# (and in the case of this introductory page, VB.NET) as keeping examples in all languages is a huge effort.



Most tutorials are implemented in the <a href="https://github.com/moonsharp-devs/moonsharp/tree/master/src/Tutorial/Tutorials">tutorial project on GitHub</a>.



<div class="alert alert-info" role="alert">
The tutorials on this site assume you know Lua *and* a managed language, preferrably C#. It's hard to follow the examples if you don't know at least the basis of Lua
and you have a good knowledge of some .NET language.
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




##### In Xamarin Studio, with Nuget

Under the "Project" menu, select "Add Nuget packages...". In the following window, search for ``MoonSharp``.
Select the project whose id is exactly ``MoonSharp``.




##### In Visual Studio (or any other IDE) without using Nuget

Refer to the documentation of your IDE to add ``MoonSharp.Interpreter.dll``, contained in the folder of the target of your choice as a dependency.




##### In Unity

>> A Unity asset store package is pending approval now and will be at <a href="https://www.assetstore.unity3d.com/#!/content/33776">this URL</a> as soon as it gets approved. As soon as it gets approved, using it becomes the recommended way to install MoonSharp in Unity. 


Put the ``MoonSharp.Interpreter.dll``, contained in the ``interpreter/net35`` folder
of the MoonSharp distribution, in your ``Assets/Plugins`` folder.

If Windows Store apps / Windows Phone support is needed, copy ``MoonSharp.Interpreter.dll`` contained 
in the ``interpreter/portable-net40`` folder of the MoonSharp distribution, in an ``Assets/Plugins/WSA`` folder.
Then, <a href="http://docs.unity3d.com/Manual/windowsstore-plugins.html">follow this guide</a>.

After this, add them as references from MonoDevelop-Unity.

Note: if you are going to use MoonSharp in IL2CPP projects, also create/edit a link.xml inside your Assets directory with this contents:

{% highlight xml %}
<linker>
    <assembly fullname="MoonSharp.Interpreter">
        <type fullname="MoonSharp.*" preserve="all" />
    </assembly>
</linker> 
{% endhighlight %}




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
	' VB.NET is not very strong at embedded newlines...
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

Now you are ready to jump at the other tutorials.




        
		
		
		


