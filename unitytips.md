---
layout: tutorial
title: Tips and tricks for Unity3D
subtitle: A collection of quick tips for those using MoonSharp on Unity
---

#### Miscellaneous recommendations

* Don't expose Unity types, if possible
* Use proxy objects whenever you can
* Use hardwiring before running on IL2CPP and/or AOT platforms
* Never expose structs, if possible



#### Initialize the script loader with one of the more explicit constructors

Use the explicit constructor of UnityAssetsScriptLoader to register all script files.
By using this little snippet at the very start of your project you don't need to add Unity own libraries to ``link.xml`` to run on IL2CPP platforms.


Example:

{% highlight csharp %}

 Dictionary<string, string> scripts = new  Dictionary<string, string>();

 object[] result = Resources.LoadAll("Lua", typeof(TextAsset));

 foreach(TextAsset ta in result.OfType<TextAsset>())
 {
     scripts.Add(ta.name, ta.text);
 }

 Script.DefaultOptions.ScriptLoader = new MoonSharp.Interpreter.Loaders.UnityAssetsScriptLoader(scripts);

{% endhighlight %}




