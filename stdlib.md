---
layout: default
title: Standard Library
subtitle: Status of the Lua standard library in MoonSharp
---

This page tracks which parts of the Lua 5.2 standard library MoonSharp implements, on which platforms, and which functions can yield.

> NOTE : this reflects the state of the standard library as captured in the original status document. It has not been re-verified against the 3.0.0 beta - see the <a href="changelog.html">changelog</a> for recent changes.

<div class="panel panel-default">
  <div class="panel-heading">Legend</div>
  <table class="table table-condensed" style="margin-bottom:0">
    <tbody>
      <tr><td style="width:12em"><span class="sl-badge sl-new">New in Moon#</span></td><td>Function has been introduced in Moon# and is not present in Lua 5.2</td></tr>
      <tr><td><span class="sl-badge sl-done">Done</span></td><td>Implemented completely</td></tr>
      <tr><td><span class="sl-badge sl-diff">Done, with differences</span></td><td>Implemented, but has differences from the Lua version (detailed in comments)</td></tr>
      <tr><td><span class="sl-badge sl-issues">Not done, issues</span></td><td>Not completed yet</td></tr>
      <tr><td><span class="sl-badge sl-stub">Unsupported, stub</span></td><td>Unsupported, but implemented as a stub for compatibility</td></tr>
      <tr><td><span class="sl-badge sl-unsup">Unsupported</span></td><td>Unsupported and likely support will be limited or non-existent also in the future</td></tr>
      <tr><td><span class="sl-badge sl-todo">Not done yet</span></td><td>Not even started</td></tr>
    </tbody>
  </table>
</div>

<p>
  The <strong>Yield</strong> column says whether the function can yield a coroutine:
  <strong>YES</strong> it can, <strong>NO</strong> it cannot, <strong>&ndash;</strong> not applicable.
</p>

<div class="form-group">
  <input type="text" id="sl-filter" class="form-control" placeholder="Filter functions... (e.g. string, io, yield)" autocomplete="off">
  <p class="help-block" id="sl-count"></p>
</div>

<div class="table-responsive">
<table class="table table-condensed table-hover" id="sl-table">
  <thead>
    <tr>
      <th>API</th>
      <th>Status</th>
      <th>Yield</th>
      <th>CLR</th>
      <th>Mono</th>
      <th>Unity</th>
      <th>Differences</th>
      <th>Yielding notes</th>
    </tr>
  </thead>
  <tbody>
    <tr><td><code>_G</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>_VERSION</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>_MOONSHARP</code></td><td><span class="sl-badge sl-new">New in Moon#</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>A table, containing several fields including moonsharp version, platform and emulated lua version.</td><td></td></tr>
    <tr><td><code>assert</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>collectgarbage</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Some arguments ignored</td><td></td></tr>
    <tr><td><code>dofile</code></td><td><span class="sl-badge sl-done">Done</span></td><td>YES</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>error</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>getmetatable</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>ipairs</code></td><td><span class="sl-badge sl-done">Done</span></td><td>YES</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>load</code></td><td><span class="sl-badge sl-done">Done</span></td><td>NO</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td>A function as input cannot yield.</td></tr>
    <tr><td><code>loadsafe</code></td><td><span class="sl-badge sl-new">New in Moon#</span></td><td>NO</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Same as <code>loadfile</code>, but with better handling of default <code>_ENV</code> for sandboxing.</td><td></td></tr>
    <tr><td><code>loadfile</code></td><td><span class="sl-badge sl-diff">Done, with differences</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>stdin as input is not supported.</td><td></td></tr>
    <tr><td><code>loadfilesafe</code></td><td><span class="sl-badge sl-new">New in Moon#</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Same as <code>loadfile</code>, but with better handling of default <code>_ENV</code> for sandboxing.</td><td></td></tr>
    <tr><td><code>next</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>pairs</code></td><td><span class="sl-badge sl-done">Done</span></td><td>YES</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>pcall</code></td><td><span class="sl-badge sl-done">Done</span></td><td>YES</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>print</code></td><td><span class="sl-badge sl-done">Done</span></td><td>NO</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td><code>__tostring</code> cannot yield when called from print</td></tr>
    <tr><td><code>rawequal</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>rawget</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>rawlen</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>rawset</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>require</code></td><td><span class="sl-badge sl-done">Done</span></td><td>YES</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Minor internal differences</td><td></td></tr>
    <tr><td><code>select</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>setmetatable</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>tonumber</code></td><td><span class="sl-badge sl-diff">Done, with differences</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Only bases between 2 and 10, and base 16 are supported.</td><td></td></tr>
    <tr><td><code>tostring</code></td><td><span class="sl-badge sl-done">Done</span></td><td>YES</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>type</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>xpcall</code></td><td><span class="sl-badge sl-diff">Done, with differences</span></td><td>NO</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Double faults are appended to error and ignored.</td><td>Code can yield, but the error message handler cannot.</td></tr>

    <tr><td><code>bit32.arshift</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>bit32.band</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>bit32.bnot</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>bit32.bor</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>bit32.btest</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>bit32.bxor</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>bit32.extract</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>bit32.lrotate</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>bit32.lshift</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>bit32.replace</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>bit32.rrotate</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>bit32.rshift</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>

    <tr><td><code>coroutine.create</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>coroutine.resume</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>coroutine.running</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>coroutine.status</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>coroutine.wrap</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>coroutine.yield</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>

    <tr><td><code>debug.debug</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>debug.getuservalue</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>debug.gethook</code></td><td><span class="sl-badge sl-unsup">Unsupported</span></td><td></td><td class="n">&#10007;</td><td class="n">&#10007;</td><td class="n">&#10007;</td><td>Use the debugger infrastructure.</td><td></td></tr>
    <tr><td><code>debug.getinfo</code></td><td><span class="sl-badge sl-unsup">Unsupported</span></td><td></td><td class="n">&#10007;</td><td class="n">&#10007;</td><td class="n">&#10007;</td><td>Function is heavily dependent on implementation details.</td><td></td></tr>
    <tr><td><code>debug.getlocal</code></td><td><span class="sl-badge sl-unsup">Unsupported</span></td><td></td><td class="n">&#10007;</td><td class="n">&#10007;</td><td class="n">&#10007;</td><td>Function is heavily dependent on implementation details.</td><td></td></tr>
    <tr><td><code>debug.getmetatable</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>debug.getregistry</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>debug.getupvalue</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>debug.setuservalue</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>debug.sethook</code></td><td><span class="sl-badge sl-unsup">Unsupported</span></td><td></td><td class="n">&#10007;</td><td class="n">&#10007;</td><td class="n">&#10007;</td><td>Use the debugger infrastructure.</td><td></td></tr>
    <tr><td><code>debug.setlocal</code></td><td><span class="sl-badge sl-unsup">Unsupported</span></td><td></td><td class="n">&#10007;</td><td class="n">&#10007;</td><td class="n">&#10007;</td><td>Function is heavily dependent on implementation details.</td><td></td></tr>
    <tr><td><code>debug.setmetatable</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Cannot set on userdata as now.</td><td></td></tr>
    <tr><td><code>debug.setupvalue</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>debug.traceback</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>debug.upvalueid</code></td><td><span class="sl-badge sl-diff">Done, with differences</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Works ok, but returns a number instead of userdata (still, can be used in pretty much the same way).</td><td></td></tr>
    <tr><td><code>debug.upvaluejoin</code></td><td><span class="sl-badge sl-todo">Not done yet</span></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>

    <tr><td><code>dynamic.eval</code></td><td><span class="sl-badge sl-new">New in Moon#</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Evaluates the code passed as a parameter dynamically. All accesses are raw, function calls will raise an error.</td><td></td></tr>
    <tr><td><code>dynamic.prepare</code></td><td><span class="sl-badge sl-new">New in Moon#</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Prepares an expression for faster evaluation with <code>dynamic.eval</code>.</td><td></td></tr>

    <tr><td><code>file:close</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>file:flush</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>file:lines</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>file:read</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>file:seek</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>file:setvbuf</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>file:write</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>

    <tr><td><code>io.close</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>io.flush</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>io.input</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>io.lines</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>io.open</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>io.output</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>io.popen</code></td><td><span class="sl-badge sl-unsup">Unsupported</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td>Interprocess comms, stdin and stdout are NOT supported. This however is ok by the Lua standard.</td><td></td></tr>
    <tr><td><code>io.read</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>io.stderr</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>io.stdin</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>io.stdout</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>io.tmpfile</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>io.type</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>io.write</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>

    <tr><td><code>math.abs</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.acos</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.asin</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.atan</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.atan2</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.ceil</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.cos</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.cosh</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.deg</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.exp</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.floor</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.fmod</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.frexp</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.huge</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.ldexp</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.log</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.max</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.min</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.modf</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.pi</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.pow</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.rad</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.random</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Minor differences in accepted inputs (MoonSharp is more tolerant)</td><td></td></tr>
    <tr><td><code>math.randomseed</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.sin</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.sinh</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.sqrt</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.tan</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>math.tanh</code></td><td><span class="sl-badge sl-done">Done</span></td><td>&ndash;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>

    <tr><td><code>os.clock</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>os.date</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Running on Mono systems might lead to erroneous output due to a Mono bug (11817)</td><td></td></tr>
    <tr><td><code>os.difftime</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>os.execute</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>os.exit</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>os.getenv</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>os.remove</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>os.rename</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>
    <tr><td><code>os.setlocale</code></td><td><span class="sl-badge sl-stub">Unsupported, stub</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td>Unsupported, currently a stub</td><td></td></tr>
    <tr><td><code>os.time</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>os.tmpname</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="n">&#10007;</td><td></td><td></td></tr>

    <tr><td><code>package.config</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>package.cpath</code></td><td><span class="sl-badge sl-unsup">Unsupported</span></td><td></td><td class="n">&#10007;</td><td class="n">&#10007;</td><td class="n">&#10007;</td><td>See <a href="scriptloaders.html">Script loaders</a> to see how to customize loading of scripts and packages</td><td></td></tr>
    <tr><td><code>package.loaded</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>package.loadlib</code></td><td><span class="sl-badge sl-unsup">Unsupported</span></td><td></td><td class="n">&#10007;</td><td class="n">&#10007;</td><td class="n">&#10007;</td><td>See <a href="scriptloaders.html">Script loaders</a> to see how to customize loading of scripts and packages</td><td></td></tr>
    <tr><td><code>package.path</code></td><td><span class="sl-badge sl-unsup">Unsupported</span></td><td></td><td class="n">&#10007;</td><td class="n">&#10007;</td><td class="n">&#10007;</td><td>See <a href="scriptloaders.html">Script loaders</a> to see how to customize loading of scripts and packages</td><td></td></tr>
    <tr><td><code>package.preload</code></td><td><span class="sl-badge sl-unsup">Unsupported</span></td><td></td><td class="n">&#10007;</td><td class="n">&#10007;</td><td class="n">&#10007;</td><td>See <a href="scriptloaders.html">Script loaders</a> to see how to customize loading of scripts and packages</td><td></td></tr>
    <tr><td><code>package.searchers</code></td><td><span class="sl-badge sl-unsup">Unsupported</span></td><td></td><td class="n">&#10007;</td><td class="n">&#10007;</td><td class="n">&#10007;</td><td>See <a href="scriptloaders.html">Script loaders</a> to see how to customize loading of scripts and packages</td><td></td></tr>
    <tr><td><code>package.searchpath</code></td><td><span class="sl-badge sl-unsup">Unsupported</span></td><td></td><td class="n">&#10007;</td><td class="n">&#10007;</td><td class="n">&#10007;</td><td>See <a href="scriptloaders.html">Script loaders</a> to see how to customize loading of scripts and packages</td><td></td></tr>

    <tr><td><code>string.byte</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Character codes are cropped to 0-255. Use <code>string.unicode</code> to have the unicode code-point.</td><td></td></tr>
    <tr><td><code>string.char</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Values &gt; 255 are silently supported (as unicode codepoints)</td><td></td></tr>
    <tr><td><code>string.dump</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Functions with upvalues raise an error as Lua 5.0 did and as Lua 5.2 should do according to documentation.</td><td></td></tr>
    <tr><td><code>string.find</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Implementation taken from KopiLua. Patterns cannot contain \0. Use %z instead.</td><td></td></tr>
    <tr><td><code>string.format</code></td><td><span class="sl-badge sl-done">Done</span></td><td>NO</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Implementation taken from KopiLua.</td><td><code>__tostring</code> metamethod cannot yield.</td></tr>
    <tr><td><code>string.gmatch</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Implementation taken from KopiLua. Patterns cannot contain \0. Use %z instead.</td><td></td></tr>
    <tr><td><code>string.gsub</code></td><td><span class="sl-badge sl-done">Done</span></td><td>NO</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Implementation taken from KopiLua. Patterns cannot contain \0. Use %z instead.</td><td>Callback function (if used) cannot yield.</td></tr>
    <tr><td><code>string.len</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>string.lower</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>string.match</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Implementation taken from KopiLua. Patterns cannot contain \0. Use %z instead.</td><td></td></tr>
    <tr><td><code>string.rep</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>string.reverse</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>string.sub</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>string.unicode</code></td><td><span class="sl-badge sl-new">New in Moon#</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td>Works just as <code>string.byte</code> would do, but returns the unicode code-point without truncation.</td><td></td></tr>
    <tr><td><code>string.upper</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>

    <tr><td><code>table.concat</code></td><td><span class="sl-badge sl-done">Done</span></td><td>NO</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td><code>__len</code> cannot yield</td></tr>
    <tr><td><code>table.insert</code></td><td><span class="sl-badge sl-done">Done</span></td><td>NO</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td><code>__len</code> cannot yield</td></tr>
    <tr><td><code>table.pack</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
    <tr><td><code>table.remove</code></td><td><span class="sl-badge sl-done">Done</span></td><td>NO</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td><code>__len</code> cannot yield</td></tr>
    <tr><td><code>table.sort</code></td><td><span class="sl-badge sl-done">Done</span></td><td>NO</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td><code>__lt</code>, <code>__len</code> and the comparison function cannot yield</td></tr>
    <tr><td><code>table.unpack</code></td><td><span class="sl-badge sl-done">Done</span></td><td></td><td class="y">&#10003;</td><td class="y">&#10003;</td><td class="y">&#10003;</td><td></td><td></td></tr>
  </tbody>
</table>
</div>

<p class="help-block">
  The original spreadsheet is still available as a <a href="MoonSharpStdLib.pdf">PDF download</a>.
</p>

<style>
.sl-badge {
	display: inline-block;
	padding: 2px 7px;
	font-size: 85%;
	line-height: 1.2;
	white-space: nowrap;
	border-radius: 3px;
	color: #000;
}
.sl-done   { background-color: #92d050; }
.sl-diff   { background-color: #ffff00; }
.sl-new    { background-color: #00ffff; }
.sl-issues { background-color: #ffc000; }
.sl-stub   { background-color: #7030a0; color: #fff; }
.sl-unsup  { background-color: #ff0000; color: #fff; }
.sl-todo   { background-color: #d9d9d9; }

#sl-table { font-size: 10pt; }
#sl-table td, #sl-table th { vertical-align: middle; }
#sl-table td.y, #sl-table td.n { text-align: center; font-weight: bold; }
#sl-table td.y { color: #4a8f1a; }
#sl-table td.n { color: #cc0000; }
</style>

<script>
(function () {
	var input = document.getElementById('sl-filter');
	var count = document.getElementById('sl-count');
	var rows = document.querySelectorAll('#sl-table tbody tr');
	var total = rows.length;

	function update() {
		var q = input.value.toLowerCase().trim();
		var shown = 0;
		for (var i = 0; i < rows.length; i++) {
			var match = q === '' || rows[i].textContent.toLowerCase().indexOf(q) !== -1;
			rows[i].style.display = match ? '' : 'none';
			if (match) shown++;
		}
		count.textContent = q === '' ? (total + ' functions') : (shown + ' of ' + total + ' functions');
	}

	input.addEventListener('input', update);
	update();
})();
</script>
