---
layout: tutorial
title: Hardwire descriptors
subtitle: helping AOT and IL2CPP since 2016
---

> If some of the terms used in this page are unknown to you, see the glossary section at the bottom.


#### Why "hardwiring"

MoonSharp, as most if not all other libraries of its kind, uses reflection and, sometimes, code generated at runtime to allow access to C# code from Lua scripts.

At its roots MoonSharp is already a lot gentler than most other scripting engines in that:

* it can run, in theory, without reflection at all (but at a great effort of writing code) which helps with speed and avoiding some headaches (IL2CPP's link.xml..)
* it doesn't generate IL code at runtime on platforms it shouldn't (iOS on Xamarin, IL2CPP platforms on Unity, etc.)
* no loss of functionality is caused by the previous point, simply it will be a little slower

This is all good and allows for a great deal of compatibility, but something more can be done. 

What if most reflection goes away (and the little that stays is quite innocuous) and get better performance to boot ? 

To good to be true ? Yes, in a sense. Here comes "hardwiring" into play, but at some costs.


#### What is "hardwiring"

The idea is simple. To avoid reflection the only way, so far, was writing a lot of glue code in C#. A lot. Of hard to write C# code. But this isn't needed - we can simplify a little bit how to write it. But it's still a boring work. Where making a mistake is easy.

But turns out that boring, hard work, where making a mistake is easy is the ideal scenario for automation.And MoonSharp has all the info it needs to do it. 

So, to cut it short, through "hardwiring" MoonSharp can create a bunch of C# code which runs without requiring further reflection on types.



#### How to hardwire

Simple. Well almost.

MoonSharp has to take a dump, somewhere, of all registered types. To do this, a little temporary snippet is required:

{% highlight csharp %}

Table dump = UserData.GetDescriptionOfRegisteredTypes(true);
File.WriteAllText(@"c:\temp\testdump.lua", dump.Serialize());

{% endhighlight %}

The important thing is that the above lines of code are run **after** all the types have been registered. So, first rule of hardwiring is to make order of all the registrations.

Now, the code can be generated using the moonsharp repl interpreter itself:

	moonsharp -W c:\temp\testdump.lua c:\temp\out.cs --internals --class:MyClass --namespace:MyNamespace

At this point, the generation will likely spit out a lot of warnings/errors. They will be reported both in the console output and as comments in the code. The generated code will work even in the presence of warnings/errors, but take your time to read them as if your script code references the members which triggered the warnings/errors the behavior could be different from the expected one.

> If you don't have a Windows computer, you can easily use mono to execute the moonsharp interpreter on Mac or Linux!

The generated code will consist of a single public static class with a single public, static, Initialize method, which you should call before doing anything else with MoonSharp. Pretty easy.

<div class="alert alert-warning" role="alert">
If you register a system type (for example, say, an array) you might get into some trouble when hardwiring. The reason is that hardwiring will use the definition of the type used in the runtime which generated the "dump", which might be different at runtime if, say, you change .NET version or platform. While this might seem like a corner case, it will happen for Windows Phone and Windows Store apps on Unity. At worst, put a conditional compilation around generated code.
</div>



#### Hardwiring Pros and Cons

Pros:

* Greatly reduced reflection usage 
* No need to whitelist types where metadata is stripped (e.g. IL2CPP)
* No runtime code generation
* Greatly improved performance, specially on platforms without runtime code generation

Cons:

* Code generation step to be put in workflow 
* Code generation generates only C# or VB.NET
* Types and members exposed must be accessible by other classes - in short, public or internal if in the same assembly
* Some types of members might be unsupported (events, for example, at least in the current version)
* Value types setters won't work .. or, well, they will work even less than usual - just avoid them

<div class="alert alert-warning" role="alert">
The hardwiring system currently covers all the types registered with UserDataType without passing a user data descriptor. Apart from custom descriptors - on which you are obviously on your own - this means that types used with PropertyTableAssigner and extension methods are NOT included. Usually this is less of a problem as they tend to be used in types which are visible by IL2CPP heuristics, and will likely be implemented in the future.
</div>



#### Is hardwiring required to get IL2CPP, AOT or iOS compatibility ?

AOT compatibility is managed through a PlatformAccessor. Actually the only behavior change is that no runtime code generation is performed and pure reflection is used on AOT platforms.

Regarding IL2CPP compatibility, hardwiring simplifies it - by removing the need to maintain a link.xml file for all the metadata which shouldn't be stripped - and makes it more efficient as reflection is slow. But you can still work your way by adding entries to link.xml.


#### Glossary

* Reflection : a set of types and methods which allow code to "inspect" itself and, eventually, invoke methods, read and write fields and properties, etc. Works well but it's slow. See <a href="https://msdn.microsoft.com/en-us/library/system.reflection(v=vs.110).aspx">MSDN docs</a>.

* Runtime code generation : the practice of generating code at runtime to optimize operations done through reflection. MoonSharp uses this in some points, but in case of AOT platforms, this gets disabled.

* IL : Code in .NET/mono assemblies is stored using IL - intermediate language bytecode. This bytecode cannot be executed directly but must be translated to native instructions before execution.

* JIT : Just-In-Time compilation. Normally, when .NET or mono load an assembly, they perform a just-in-time compilation of IL code, translating it to native code. This can happen also later in the lifetime of an assembly - for example generic types are usually re-compiled on the fly if one of their type parameters is a value-type.

* AOT : Ahead-of-time compilation. An option that mono offers (and in a way also ngen and .NET native, but they are off topic to this discussion, at least for now) is to compile the code to native code or another representation ahead of time. This is not trivial at all and might fail to compile all the code required. For example if reflection is used to instantiate a type which is never referenced in the code, that particular piece of code might not have been compiled. AOT execution is required to run on iOS devices.

* IL2CPP : A marvelous piece of software which translates IL to C++ sources. It's the typical piece of software which is uber-hard to write and yet most people talking about it will do that for complaining :). Seriously the problem it tries to solve is extremely hard and a little bit of cooperation is needed to ensure compatibility. In particular, in addition to all AOT issues (AOT is mandatory for IL2CPP) IL2CPP performs opt-out stripping of assemblies metadata which might interfere with reflection. IL2CPP is mandatory for iOS, tvOS and webGL under Unity3D and available for many more platforms as an option.

* link.xml : A file which tells IL2CPP which types it should preserve the metadata of.

* Value-type : A value-type is a type which is passed by value instead of reference. It's composed of numeric primitive types, enumerations and structs. When talking about value-type issues, we are referring mostly to structs. See <a href="https://msdn.microsoft.com/en-us/library/ah19swz4.aspx">structs on MSDN</a> . Best practices say <a href="https://msdn.microsoft.com/en-us/library/ms229017(v=vs.110).aspx">value types should be immutable</a>. That is, their fields and properties should be read-only, and if you need to change them, you should create a new object (which in the case of structs is very cheap, btw). MoonSharp does NOT work well with mutable structs because, well, they are an oddity which don't work well with a lot of things, so please don't use them. If you must, consider hacking around with proxy objects. 














