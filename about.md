---
layout: home
---

MoonSharp is a Lua interpreter written entirely in C# for maximum compatibility in .NET, Mono, Xamarin and Unity.

It's a "clean room" design - that is, almost none of the original source code has been reused (apart from parts of the standard
library) and most of it has been developed by studying the specifications and the behavior of the official implementations.



#### Features
* 99% compatible with Lua 5.2 (with the only unsupported feature being weak tables support) 
* Support for metalua style anonymous functions (lambda-style)
* Easy to use API
* Source based remote **debugger** accessible with a web browser and Flash (PCL targets not supported)
* Runs on .NET 3.5, .NET 4.x, Mono, Xamarin and Unity3D
* Runs on Ahead-of-time platforms like iOS
* Runs on platforms requiring a .NET 4.x portable class library (e.g. Windows Phone)
* Easy and performant interop with CLR objects, with runtime code generation where supported
* Interop with methods, extension methods, overloads, fields, properties and indexers supported
* Support for the complete Lua standard library with very few exceptions (mostly located on the 'debug' module)
* Async methods for .NET 4.x targets
* Supports dumping/loading bytecode for obfuscation and quicker parsing at runtime
* Easy opt-out of Lua standard library modules to sandbox what scripts can access
* Easy to use error handling (script errors are exceptions)
* Support for coroutines, including invocation of coroutines as C# iterators 
* REPL interpreter, plus facilities to easily implement your own REPL in few lines of code
* Complete XML help, and walkthroughs on http://www.moonsharp.org



#### Project Status

* Completed, barring improvements, enhancements and new features!
* Some intentional minimal differences exist - [see here](moonluadifferences.html)
* All Lua 5.2 language constructs are implemented, except weak tables.
* Development of the standard library is finished. Updated situation in [this pdf](http://www.moonsharp.org/MoonSharpStdLib.pdf)
 

##### Roadmap

* Language analysis features
* Improvements
 

 
#### License

The program and libraries are released under a 3-clause BSD license - see the license section.

Some parts of the string library (parttern matching and string.format) are taken from [KopiLua](https://github.com/NLua/KopiLua)
