---
title: dotNet DOS command in Maxscript
tags:
  - 3ds Max
  - Maxscript
  - DotNet
---

Just a quick useful snippet - occasionally it can be necessary to be able to call out to the command line from Maxscript for some task. This might be to run a Python script that atlases a folder of images you've just batch rendered, or a script that uploads asset data to your companies Shotgun database... etc. There are a whole bunch of reasons you might find you need to do this given the relative limitations of Maxscript.

{% highlight shell-session %}
DOSCommand <command_string>
{% endhighlight %}

The two (roughly equivalent) functions we're interested in being `DOSCommand` and `HiddenDOSCommand` ([doc link](http://help.autodesk.com/view/3DSMAX/2016/ENU/?guid=__files_GUID_846B6AB0_EFF6_43E5_8A67_8D348FF78A57_htm)). The limitation we have is that any output from our actions at the command prompt are lost and all we're left with in Max is an integer error code.

But if we use the dotNetObject "System.Diagnostics.Process" instead, we can redirect any output or errors from the command line back to be handled in Maxscript. I've not used that much dotNet in Maxscript so far, so this is a pretty simple implementation and I won't go into details. All the code for an alternative to `DOSCommand` is below.

#### Gist
<script src="https://gist.github.com/ross-g/38696ac49da9ee0d8d4a.js"></script>

#### Usage
This runs a script or executable from the command prompt with an optional array of string arguments. (format these as for the command line) The script will capture any command prompt output (including errors) and return it as an array of strings to Maxscript (or optionally returned as a single string with newline and return characters).

eg
{% highlight shell-session %}
python = SystemTools.GetEnvVariable "PYTHONPATH"
Dos_Command.run python arg_array:#("C:\path\to\script.py", "--argument 1.0", "-verbose")
{% endhighlight %}
