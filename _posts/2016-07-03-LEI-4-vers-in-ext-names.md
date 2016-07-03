---
layout: post
title:  LEI 4 - Version Numbers in Extension Names
---

Each LANDIS-II extension is distributed with a text file containing information about that extension.
For example, here’s the [info file][] for the current version (4.1) of [Age-only Succession][]:

    LandisData   Extension
    
    Name         "Age-only Succession"
    Version      4.1
    Type         succession
    Assembly     Landis.Extension.Succession.AgeOnly
    Class        Landis.Extension.Succession.AgeOnly.PlugIn
    Description  "Succession with age cohorts"
    CoreVersion  6.0

The model core uses the information in these files to map extension `Name`s to `Assembly` files with executable code.
So, when the model encounters the extension name `Age-only Succession` in a scenario file, it loads the extension’s code from the assembly file named `Landis.Extension.Succession.AgeOnly.dll`.

[info file]:  https://github.com/LANDIS-II-Foundation/Extensions-Succession/blob/master/age-only-succession/trunk/deploy/Age-only%20Succession%204.1.txt
[Age-only Succession]:  http://www.landis-ii.org/extensions/age-only-succession

Unfortunately, the current convention for extension and assembly names has a significant limitation -- _only one version of an extension can be installed at a time._
For example, since all versions of Age-only Succession (version 4.1 and all its predecessors) have information files with the same `Name` and `Assembly` fields, only one of these versions can be installed and used at a time.
This limitation makes it very difficult for users to compare different versions of an extension.

To illustrate this, suppose a bug is discovered in the current version of an extension.
Developers fix it and release a new version.
Naturally, users will need to verify that the buggy behavior has been fixed for their particular scenarios.
So they:

1. run their scenarios with the old version of the extension,
2. install the new version,
3. re-run the scenarios with the new version, and finally
4. compare results.

If their initial analysis reveals that they need to run more scenarios with both versions to confirm the bug fix, they have to use a [2-step process to restore the previous version][]:

1. uninstall the new version, and then
2. re-install the previous version.

[2-step process to restore the previous version]:  https://groups.google.com/d/msg/landis-ii-developers/PpkrpR9wi5Q/tDZFlfXEKs8J

This manual process is required because the logic in extension installers is designed to only _upgrade_ extensions (i.e., replace an older version with a newer one).
So in order to switch between different extension versions in scenarios, users have to uninstall one version, and then install another.
A very labor-intensive process, for sure.

Yet, there’s no technical aspect of the LANDIS-II model that prevents multiple versions of an extension from being installed concurrently (i.e., side-by-side).
All that’s needed is a __better naming scheme__.

We pioneered such a scheme for the [Land Use extension][] which we released last year.
Here’s the info file for the extension's current version:

    LandisData   Extension
    
    Name         "Land Use 1.1"
    Version      "1.1 (official release)"
    Type         "disturbance:land use"
    Assembly     Landis.Extension.LandUse-1.1
    Class        Landis.Extension.LandUse.Main
    Description  "Land Use and Land Cover Change"
    UserGuide    "Land Use v1.1 - User Guide.pdf"
    CoreVersion  6.0

Note that the `Name` and `Assembly` fields __contain the major and minor version numbers.__
By including these numbers in those fields, it’s possible to install multiple versions of Land Use side-by-side.
This new naming scheme greatly improved the productivity of our development process.
Because our team had multiple versions of the extension installed concurrently, we could easily run scenarios with different versions, and compare results to verify fixes and improvements in each new release.

[Land Use extension]: http://www.landis-ii.org/extensions/land-use-change

If this naming convention with version numbers is adopted across the L-II community for all extensions, users would gain much more flexibility in switching between different versions of any extension when running their scenarios.

Furthermore, there'd be a big gain for reproducability.
One positive side-effect of this naming scheme with the Land Use extenson is now every scenario's log file (`Landis-log.txt`) contains the specific version of Land Use that was used to generate that scenario's output files.

![Landis-log.txt file with extension loading highlighted](/images/land-use-log.png)

So if every extension followed this scheme, then users would have an easier time reproducing simulation results because those results -- the output files including the log -- now document which versions of the extensions were actually used by the model to produce that particular set of output files.

There is one caveat with adopting this naming convention for all extensions -- it requires a corresponding convention for extension _libraries_ (which I’ll explain in my next post).
