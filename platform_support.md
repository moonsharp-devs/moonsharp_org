---
layout: default
title: Platform support
subtitle: 
---

Supported features varies (wildly) by platform. Here is a small recap of differences you might expect, but the list is constantly evolving so don't take it 100% for granted.

<table>
    <tr>
        <th>Feature</th>
        <th>.NET 3.5</th>
        <th>.NET 4.x</th>
        <th>.NET Core</th>
        <th>PCL library</th>
        <th>WSA Apps (1)</th>
        <th>Unity Mono</th>
        <th>Unity IL2CPP (2)</th>
        <th>Unity AOT (3)</th>
        <th>Unity WSA Apps</th>
    </tr>
    <tr>
        <th>Script execution</th><!-- Feature -->
        <td>Y</td><!-- .NET 3.5 -->
        <td>Y</td><!-- .NET 4.x -->
        <td>Y</td><!-- .NET Core -->
        <td>Y</td><!-- PCL library -->
        <td>Y</td><!-- WSA Apps (1) -->
        <td>Y</td><!-- Unity Standalone -->
        <td>Y</td><!-- Unity IL2CPP (2) -->
        <td>Y</td><!-- Unity AOT (3) -->
        <td>Y</td><!-- Unity WSA Apps -->
    </tr>
    <tr>
        <th>Bytecode dumping</th><!-- Feature -->
        <td>Y</td><!-- .NET 3.5 -->
        <td>Y</td><!-- .NET 4.x -->
        <td>Y</td><!-- .NET Core -->
        <td>N</td><!-- PCL library -->
        <td>N</td><!-- WSA Apps (1) -->
        <td>Y</td><!-- Unity Standalone -->
        <td>Y</td><!-- Unity IL2CPP (2) -->
        <td>Y</td><!-- Unity AOT (3) -->
        <td>N</td><!-- Unity WSA Apps -->
    </tr>
    <tr>
        <th>``dynamic``</th><!-- Feature -->
        <td>N</td><!-- .NET 3.5 -->
        <td>Y</td><!-- .NET 4.x -->
        <td>N</td><!-- .NET Core -->
        <td>Y</td><!-- PCL library -->
        <td>N</td><!-- WSA Apps (1) -->
        <td>N</td><!-- Unity Standalone -->
        <td>N</td><!-- Unity IL2CPP (2) -->
        <td>N</td><!-- Unity AOT (3) -->
        <td>N</td><!-- Unity WSA Apps -->
    </tr>
    <tr>
        <th>Async APIs</th><!-- Feature -->
        <td>N</td><!-- .NET 3.5 -->
        <td>Y</td><!-- .NET 4.x -->
        <td>Y</td><!-- .NET Core -->
        <td>Y</td><!-- PCL library -->
        <td>N</td><!-- WSA Apps (1) -->
        <td>N</td><!-- Unity Standalone -->
        <td>N</td><!-- Unity IL2CPP (2) -->
        <td>N</td><!-- Unity AOT (3) -->
        <td>N</td><!-- Unity WSA Apps -->
    </tr>    
    <tr>
        <th>Optimized interop</th><!-- Feature -->
        <td>Y</td><!-- .NET 3.5 -->
        <td>Y</td><!-- .NET 4.x -->
        <td>Y</td><!-- .NET Core -->
        <td>Y</td><!-- PCL library -->
        <td>Y</td><!-- WSA Apps (1) -->
        <td>Y</td><!-- Unity Standalone -->
        <td>N</td><!-- Unity IL2CPP (2) -->
        <td>N</td><!-- Unity AOT (3) -->
        <td>Y</td><!-- Unity WSA Apps -->
    </tr>  
    <tr>
        <th>VsCode debugger</th><!-- Feature -->
        <td>Y</td><!-- .NET 3.5 -->
        <td>Y</td><!-- .NET 4.x -->
        <td>Y</td><!-- .NET Core -->
        <td>N</td><!-- PCL library -->
        <td>N</td><!-- WSA Apps (1) -->
        <td>Y</td><!-- Unity Standalone -->
        <td>N</td><!-- Unity IL2CPP (2) -->
        <td>N</td><!-- Unity AOT (3) -->
        <td>N</td><!-- Unity WSA Apps -->
    </tr>
    <tr>
        <th>Remote debugger</th><!-- Feature -->
        <td>Y</td><!-- .NET 3.5 -->
        <td>Y</td><!-- .NET 4.x -->
        <td>N</td><!-- .NET Core -->
        <td>N</td><!-- PCL library -->
        <td>N</td><!-- WSA Apps (1) -->
        <td>N</td><!-- Unity Standalone -->
        <td>N</td><!-- Unity IL2CPP (2) -->
        <td>N</td><!-- Unity AOT (3) -->
        <td>N</td><!-- Unity WSA Apps -->
    </tr>                 
     <tr>
        <th>IO and OS libraries (4)</th><!-- Feature -->
        <td>Y</td><!-- .NET 3.5 -->
        <td>Y</td><!-- .NET 4.x -->
        <td>(5)</td><!-- .NET Core -->
        <td>Y</td><!-- PCL library -->
        <td>N</td><!-- WSA Apps (1) -->
        <td>(6)</td><!-- Unity Standalone -->
        <td>N</td><!-- Unity IL2CPP (2) -->
        <td>N</td><!-- Unity AOT (3) -->
        <td>N</td><!-- Unity WSA Apps -->
    </tr>
     <tr>
        <th>Support for value types (7)</th><!-- Feature -->
        <td>Y</td><!-- .NET 3.5 -->
        <td>Y</td><!-- .NET 4.x -->
        <td>Y</td><!-- .NET Core -->
        <td>Y</td><!-- PCL library -->
        <td>Y</td><!-- WSA Apps (1) -->
        <td>Y</td><!-- Unity Standalone -->
        <td>N</td><!-- Unity IL2CPP (2) -->
        <td>N</td><!-- Unity AOT (3) -->
        <td>Y</td><!-- Unity WSA Apps -->
    </tr>
</table>


1 - WSA Apps should use the PCL DLL included on NuGet and in the distribution Zip. This column refers to WSA apps built using the sources
2 - Unity IL2CPP includes most targets and is the recommended platform if you can't get the Mono runtime working
3 - Unity AOT column refers to the Ahead of Time runtime without IL2CPP. It's usage is not recommended 
4 - Support of ``io`` and ``os`` modules has known bugs. Beside them, known bugs exist in timezone conversions when running on Mono (due to bugs in the mono libraries)
5 - .NET Core support of ``io`` and ``os`` modules is complete except for ``os.exec`` and the issues mentioned before.
6 - Unity standalone player support for ``io`` and ``os`` modules is disabled by default but can be forced on and be fully working. Their use is still discouraged.
7 - Value types are an endless source of pain on every platform (due to how the poorly support reflection), but on IL2CPP and AOT platforms a lot more issues can be expected


