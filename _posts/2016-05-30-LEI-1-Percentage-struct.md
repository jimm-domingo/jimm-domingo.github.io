---
layout: post
title:  LEI 1 - Change Percentage to Struct
---

Prior to LANDIS-II, I worked on other simulation models for natural resources.
Over two decades of developing all these models, I've seen various ways of handling input parameters.
One of the most intriguing type of input parameters is _percentages_.

They're tricky little buggers because we developers -- to make things easier on ourselves -- have users enter them as numbers using one of 2 notations:

*  With an __implied % sign__ -- so the user enters 10% as "10".

*  Or, as their __decimal equivalents__ -- so the user enters 10% as "0.1".

The drawback with either notation is their ambiguity.
Consider this input parameter:

    PercentHarvested  1

Which notation is it?
Is the % sign implied, so just 1% is harvested?
Or, is it the decimal equivalent (i.e., 1 = 1.0 = 100%), so everything is harvested?

It's not immediately obvious, so users have to consult the documentation.
And we all know how often they do that. [^1]

Naturally, if it's an input file that the user has worked with a lot, they're going to know which notation is being used.
But a new user won't know.
And even an experienced user who originally created the input file may return to the file after some time away on other tasks, and not recall which percentage notation is being used.

When developing the code in LANDIS-II's [utility library][] for input parameters, I realized that we needed an input data type (like Int, String, Double) for percentages with an __explicit % sign__.
That way, users would never confuse these 2 examples:

    PercentHarvested   1%
    PercentHarvested 100%

That led me to design the [Percentage][] data type.

[utility library]: https://github.com/LANDIS-II-Foundation/Core-Utilities-Library
[Percentage]: https://github.com/LANDIS-II-Foundation/Core-Utilities-Library/blob/master/src/Percentage.cs

It's essentially a Double with a trailing % sign.
The `Percentage.Parse` method requires that the sign is present.
Therefore, users must specify percentages for input parameters with an unambiguous notation.
There's no doubt whether the intent is to harvest just one percent or everything.

On the output side of things, the `Percentage.ToString` method includes the trailing % sign.
This benefits not only users by clearly labelling percentages in output files, but also developers when they're debugging code.
Here's how a Percentage value appears in the debugger:

![Screenshot of a percentage variable in the Locals windows of the C# debugger](/images/percentage-debugger.png)

This all sounds great, so what needs improved?

Percentage is a _reference_ data type, i.e., a class.
To be honest, I don't recall why I did that.
Mostly likely inexperience -- LANDIS-II was my first C# project.
Perhaps, I thought it needed to be a class because it implements some interfaces (IComparable, IFormattable).
But there are several system _value_ types (Int, Double) that also implement those interfaces.
So who knows.

Anyhow, it's clear that Percentage is just a special case of a Double that has extra input and output handling.
But when using a Percentage value in calculations, it's used just like any other Double.

Therefore, the better design is to have __Percentage be a _value_ data type__, i.e., __a struct__.
As a class now, it currently incurs extra overhead for storing its double value on the heap.
There's no need to use the heap for a Percentage value -- it can be stored more efficiently on the stack like other value types because it's immutable and the same size as a Double.

The rest of the data type's API would not be affected.
Consequently, all the LANDIS-II code using the Percentage data type would not need to be modified.
Therefore, changing Percentage to a struct is backward-compatible at the _source_ level.

However, the change is not backward-compatible at the _binary_ level.
All the client code that uses the data type would have to be re-compiled.
This is necessary because when the C# compiler compiles code that uses a data type defined in another assembly, it records whether that type is a reference type (class) or a value type (struct).
So changing Percentage to a struct breaks binary compatibility because previously compiled L-II assemblies expect the data type to be a class.

Therefore, the biggest impact of this change would be on scheduling.
Because it's not binary backward-compatible, the change would have to be released in the next major version of the utility library.
Since the current version of the library (the [Edu.Wisc.Forest.Flel.Util.dll][] [^2] assembly) is 1.1.400.0 [^3], this change would have to be scheduled for version 2.0.

[Edu.Wisc.Forest.Flel.Util.dll]: https://github.com/LANDIS-II-Foundation/Core-Model/tree/master/model/trunk/third-party/FLEL/util/bin

And because this library is shipped with LANDIS-II, upgrading its assembly from 1.1 to 2.0 has to be coordinated with the model's release schedule.
Specifically, the upgrade would have to be scheduled for the model's next major version, i.e., LANDIS-II 7.0.


_Footnotes_

[^1]: The acronym RTFM exists for a reason.
[^2]: Yeah, that's an odd name for an assembly and namespace, but that's a [story for another time][].
[^3]: This version number is an artifact from our original [NAnt][]-based build system, yet another story for a future post.

[NAnt]: http://nant.sourceforge.net/
[story for another time]:  /2016/06/20/LEI-3-util-lib/
