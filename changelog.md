---
layout: default
title: Change Log
subtitle: (since 0.5.0)
---

#### Coming soon 
**Available on master branch** <span class="label label-warning">Unreleased</span>

* Fixed a bug where strings containing \<CR><LF> or \<LF> were not parsed correctly.
* Improved syntax error messages
* Made ':' calls work the same as '.' calls on userdata
* Partial string.format implementation.


#### Version 0.5.5 
**Released on 2014-10-08** <span class="label label-success">New</span>

* Optimized parsing stage
* Performance statistics for diagnostics

<hr />

#### Version 0.5.3 
**Released on 2014-10-02**

* Fixed a number of bugs in string library
* Fixed a nasty bug where the code accessed a "C:\temp" directory, likely failing
* Added a bunch of helpful overloads and helper methods

<hr />

#### Version 0.5.0 
**Released on 2014-09-30**

* Support for metatables
* pcall
* Standard library
* Coroutines
* Marshalling of C# objects as userdata



