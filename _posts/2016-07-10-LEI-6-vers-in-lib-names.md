---
layout: post
title:  LEI 6 - Version Numbers in Library Names
---

In [LEI 4][], I described the enhanced naming scheme that we pioneered with the Land Use extension to enable multiple versions of that extension to be installed concurrently (i.e., side-by-side).
In order to adopt that scheme across the L-II community for all extensions, a corresponding scheme for their libraries also needs to be adopted concurrently (pun intended).

[LEI 4]:  https://jimm-domingo.github.io/2016/07/03/LEI-4-vers-in-ext-names/

Like the current naming scheme for extensions, the current naming scheme for extension libraries also has a significant limitation -- _only one version of a library can be installed at a time_.
For example, all the versions of the [Succession Library][] -- the code shared by all the succession extensions -- use the same assembly name: `Landis.Library.Succession.dll`.

[Succession Library]: https://github.com/LANDIS-II-Foundation/Library-Succession

However, unlike the new scheme recommended in LEI 4 for extension names, we don’t need distinguish between _minor_ versions of libraries because of API compatibility.
The Semantic Versioning specification mentioned in [LEI 5][] ensures that minor versions of a library are backward compatible.
This allows a new minor version to be deployed with one extension, and ensures that this new version can be used by other extensions that were built with previous minor versions.

[LEI 5]:  https://jimm-domingo.github.io/2016/07/06/LEI-5-semver-libs/

For example, the current version of the Succession Library is [version 4.0][] as of this post.
Suppose a bug is discovered in this version.
The library developers fix it, and then prepare a new release of the library.
If the bug fix doesn’t alter the library’s API, or it only _extends_ the API (i.e, the new API remains backward compatible with the API in Succession Library 4.0), then the new release would be version 4.1 (per Semantic Versioning).

[version 4.0]:  https://github.com/LANDIS-II-Foundation/Library-Succession/blob/52999c66085d516c9f385fdf77b5e8091987ee0a/succession-library/trunk/src/Properties/AssemblyInfo.cs

So once Succession Library 4.1 is released, the developers of one of the succession extensions (let's say Biomass Succession) includes the library in a new version of their extension.
After a user installs this new version of Biomass Succession, _all the other succession extensions already installed on that user's computer will also use the fixed library code._
And even if the user installs another succession extension later on -- an extension that still includes Succession Library 4.0 -- the installer won’t overwrite the newer version of the library.
It knows to leave Succession Library 4.1 in place.

This example illustrates a major benefit of the API compatibility of Semantic Versioning.
This compatibility eliminates the need to update all the succession extensions at the same time for a new minor version of the Succession Library.
A synchronized update like that would be a major coordination challenge.
Instead, the developer team for each succession extension can continue with their own development schedule for their next release.
They’re able to incorporate Succession Library 4.1 into their extension on their own time frame.

Continuing with our example, suppose another bug is found in this latest version of the Succession Library.
But this new bug requires a fix that can't preserve the API’s backward compatibility.
In accordance with Semantic Versioning, this bug fix has to released in a new _major_ version of the library.
So instead of releasing version 4.2, the library developers release Succession Library 5.0.

Now, the limitation of the current library naming scheme comes into play.
If the user installs a succession extension with Succession Library 5.0, then any other installed succession extensions that expect `Landis.Library.Succession.dll` to be version 4.0 or 4.1 will __stop working.__

So, we need a better naming scheme that enables different major versions of libraries to co-exist side-by-side.
We developed such a naming scheme for library assemblies during our work on the Land Use extension.
That work resulted in 3 new [harvest-related libraries][]:

* [Site Harvest library][]
* [Biomass Harvest library][]
* [Harvest Management library][]

[harvest-related libraries]:  https://github.com/LANDIS-II-Foundation/Libraries/wiki/HarvestLibraries
[Site Harvest library]:  https://github.com/LANDIS-II-Foundation/Library-Site-Harvest
[Biomass Harvest library]:  https://github.com/LANDIS-II-Foundation/Library-Biomass-Harvest
[Harvest Management library]:  https://github.com/LANDIS-II-Foundation/Library-Harvest-Mgmt

These libraries were developed from the Base and Biomass Harvest extensions, which now share 2 of the libraries with the Land Use extension:

![architecture diagram for harvest libraries](https://raw.githubusercontent.com/wiki/LANDIS-II-Foundation/Libraries/images/HarvestLibraries_1.png)

The new naming scheme for libraries includes the major version number (_X_ in the [Semantic Versioning][] specification) in their assembly names.
For example:

* `Landis.Library.SiteHarvest-v1.dll`
* `Landis.Library.BiomassHarvest-v1.dll`
* `Landis.Library.HarvestManagement-v1.dll`

[Semantic Versioning]:  http://www.semver.org/

As prescribed in Semantic Versioning, the harvest libraries had _X_ = 0
while their initial public APIs were under development.
So their assemblies had `-v0` in their names for releases 0.1, 0.2, ….
Once their APIs stabilized for the first public release, their major versions advanced to 1, and their assembly names included`-v1`.

By including `-vX` in assembly file names, it’s possible to have different major versions of a library installed side-by-side.
