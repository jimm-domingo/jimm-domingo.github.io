---
layout: post
title:  LEI 3 - Rename the Utility Library
---

In my [LEI 1][] post, I mentioned that the LANDIS-II utility library has an odd name for its assembly and namespace: `Edu.Wisc.Forest.Flel.Util`.
Why doesn't its name start with `Landis.Library` like other libraries?

[LEI 1]:  /2016/05/30/LEI-1-Percentage-struct/

Well, the `Landis.Library` namespace contains libraries that are specific to LANDIS-II.
Because those libraries are designed for the model's extensions, they depend upon the L-II core.
But the utility library contains functionality that’s not specific to L-II.
In fact, a lot of its classes for input parameters are based on C++ code that I developed earlier in my career prior to joining the L-II community.
Since the utility library is independent of LANDIS-II, it can reside outside the `Landis` namespace.

I wanted the library's namespace to reflect its origins in the [Forest Landscape Ecology Lab][] (FLEL) at the University of Wisconsin where I was working in 2004.
At that time, the [naming guidelines][] for NET Framework 1.1 recommended this convention for namespaces:

    CompanyName.TechnologyName[.Feature][.Design]

I did consider `Wisconsin` for the library's top-level namespace.
But naming our utility library `Wisconsin.Utility` would be problematic if another group at the U also released their utility code under the same name.
So I wondered -- was there an alternative approach that would enable the library's name to include "FLEL"?

[Forest Landscape Ecology Lab]:  /about/#flel
[naming guidelines]:  https://msdn.microsoft.com/en-us/library/893ke618(v=vs.71).aspx

When I researched other naming schemes, I came across a NET open source project that had used the naming convention for Java packages.
That convention uses an organization's Internet domain in reverse order.
Lacking any other inspiration, I went with that approach, using the lab’s web address at the time (`flel.forest.wisc.edu`).

In hindsight, I’ll readily admit it was far from the best naming choice.
Of course, my only defense is:

> “There are only two hard things in Computer Science: cache invalidation and naming things.”
>
> -- [Phil Karlton](http://www.meerkat.com/karlton/)

So, what would be a more appropriate name for the utility library?

A couple years ago at Notre Dame, Alex and I collaborated with ecologists at Purdue University and the US Forest Service on a new seed dispersal algorithm for LANDIS-II.
In the project's first phase, a student programmer at Purdue coded the algorithm to run independently of LANDIS-II for initial development and testing.
In the second phase, we integrated the new seeding library into LANDIS-II's succession library.
Alas, we discovered that he had selected an even more unconventional name than the utility library's.
The new library's namespace and assembly had different names -- the former with an underscore, and the latter with a space! (_Don’t try this at home kids)_

So, what would be a more appropriate name for this component?

Following [current naming guidelines][] for NET Framework 4.5:

    Company.(Product|Technology)[.Feature][.Subnamespace]
   
I recommended `Purdue.Landis.DemographicSeeding`.
The top-level namespace ("Company") recognizes the university where the work came from.
The second-level namespace ("Product") reflects that the code arose out of work affiliated with LANDIS-II, and that it can be used separately from the model.

[current naming guidelines]:  https://msdn.microsoft.com/en-us/library/ms229026(v=vs.110).aspx

Applying this same approach to our utility library would update its name to:

     Wisconsin.Landis.Utility

Since this is clearly a backward-incompatible change, it would have to occur with the library’s next major version, 2.0, as described in [LEI 1][].
