---
layout: tutorial
title: Error handling
subtitle: .. and error generation
---

MoonSharp generates a few types of exceptions:

* ``InternalErrorException`` 
* ``SyntaxErrorException``
* ``ScriptRuntimeException``
* ``DynamicExpressionException``


all of these inherit from a common ``InterpreterException`` type, which can be used to group these exceptions together in filters.

Of course, other types of exceptions could be generated, due to bugs in calling code or MoonSharp code, but not (in theory in at least) due to errors in script code.

They also support a ``DecoratedMessage`` property which contains the error message, decorated with the reference in the source code which generated the error (if available).

#### InternalErrorException

An ``InternalErrorException`` is thrown when the interpreter encounters an internal error. 
There's not really much which can be done, usually this should be considered a fatal error of the scripting engine. 
Please report in the forums if it happens, with all possible information to reproduce it, if possible.

#### ``SyntaxErrorException``

A ``SyntaxErrorException`` is thrown when the parser cannot parse the script code or the script code is invalid for some reason.

Note that this is thrown when calling one of the ``Script`` methods (``DoFile``, ``RunString``, etc.). If the load function of the standard Lua library is called on a broken script, a ``ScriptRuntimeException`` is generated, wrapping the ``SyntaxErrorException``.

#### ScriptRuntimeException

This is the most common exception of all and, probably, the most important.

Everytime a Lua error is raised (through a call to error function, because of a runtime error, etc.) a ``ScriptRuntimeException`` is raised. For example:

{% highlight csharp %}

static void ErrorHandling()
{
	try
	{
		string scriptCode = @"    
			return obj.calcHypotenuse(3, 4);
		";

		Script script = new Script();
		DynValue res = script.DoString(scriptCode);
	}
	catch (ScriptRuntimeException ex)
	{
		Console.WriteLine("Doh! An error occured! {0}", ex.DecoratedMessage);
	}
}

{% endhighlight %}

will print:

	Doh! An error occured! chunk_1:2: attempt to index a nil value
	
If we want to raise an error to Lua, we can just do the same, and raise a ``ScriptRuntimeException``. Easy.

{% highlight csharp %}

static void DoError()
{
	throw new ScriptRuntimeException("This is an exceptional message, no pun intended.");
}


static string ErrorGen()
{
	string scriptCode = @"    
		local _, msg = pcall(DoError);
		return msg;
	";

	Script script = new Script();
	script.Globals["DoError"] = (Action)DoError;
	DynValue res = script.DoString(scriptCode);
	return res.String;
}

{% endhighlight %}

This will return:

	This is an exceptional message, no pun intended.

#### DynamicExpressionException

An ``DynamicExpressionException`` is thrown when the interpreter encounters an error evaluating a dynamic expression. See the tutorial on dynamic expressions to 
see what this means.
 
