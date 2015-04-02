---
layout: tutorial
title: Platform Accessors
subtitle: How to change the way MoonSharp interacts with the OS
---

> This tutorial will be brief and assumes that you've followed the tutorial on script loaders to the end.
>
> If you didn't, please go back and learn everything about script loaders first.

Platform accessors are very similar to script loaders although they serve a different purpose.
Platform accessors are meant to provide access to OS APIs to suit the standard library or other accessory parts of MoonSharp.
In particular, the ``io``, ``file`` and ``os`` modules heavily depend on platform accessors, but other methods go through the platform accessor, like
``print``, ``debug.debug`` and others.


#### A quick tour of predefined platform accessors

Depending on the platforms you run to you have these choices of platform accessors :

* ``StandardPlatformAccessor`` : implements all the needed methods, going to files, environment variables, etc.
* ``LimitedPlatformAccessor`` : very limited support. Disables the ``io``, ``file`` and parts of the ``os`` modules. 

The default script loader used by MoonSharp if no redefinition happens is ``LimitedPlatformAccessor`` for the portable class library build and ``StandardPlatformAccessor`` for
the other builds.


#### Changing the platform accessor

Changing the platform accessor impacts all scripts, both created or not. Because of this, changing the platform accessor should **NEVER** be done once a script has been
created. For the sake of this tutorial we'll do it anyway, but please don't do it.


{% highlight csharp %}
static void ChangePlatform()
{
	// This prints "function"
	Console.WriteLine(Script.RunString("return type(os.exit);").ToPrintString());

	// Save the old platform
	var oldplatform = Script.GlobalOptions.Platform;

	// Changing platform after a script has been created is not recommended.. do not do it.
	// We are doing it for the purpose of the walkthrough..
	Script.GlobalOptions.Platform = new LimitedPlatformAccessor();

	// This time, this prints "nil"
	Console.WriteLine(Script.RunString("return type(os.exit);").ToPrintString());

	// Restore the old platform
	Script.GlobalOptions.Platform = oldplatform;
}
{% endhighlight %}


#### Implementing your own platform accessor

You can implement your own platform accessor to define how some functions behave.

As for script loaders you have two main options: extend ``PlatformAccessorBase`` (recommended) or implement ``IPlatformAccessor``.

To take decisions about the real platform you are running on, see the ``PlatformAutoDetector`` which performs a lot of runtime checks to try to understand
which platform it's running on.


#### Customizing some aspects of the platform accessor

There are a few aspects of a platform accessor which are often going to be customized. To avoid having to implement a custom platform accessor everytime you want
to redefine how ``print`` behaves, some of these are exposed in script options. See the next chapter about script options.




















