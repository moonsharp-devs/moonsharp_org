---
layout: tutorial
title: FAQ / Recipes
subtitle: Simple examples for simple but common problems
---

#### How can I redirect the output of the *print* function ?

{% highlight csharp %}

script.Options.DebugPrint = s => { Console.WriteLine(s); }

{% endhighlight %}


#### How can I redirect the input to the Lua program ?

{% highlight csharp %}

script.Options.DebugInput = () => { return Console.ReadLine(); }

{% endhighlight %}


#### How can I redirect the IO streams of a Lua program ?

{% highlight csharp %}

IoModule.SetDefaultFile(script, Platforms.StandardFileType.StdIn, myInputStream);
IoModule.SetDefaultFile(script, Platforms.StandardFileType.StdOut, myOutputStream);
IoModule.SetDefaultFile(script, Platforms.StandardFileType.StdErr, myErrorStream);

{% endhighlight %}


#### How can I limit the number of instructions a script will execute without losing state ?

See <a href="coroutines.html#preemptive">Preemptive coroutines</a>.


