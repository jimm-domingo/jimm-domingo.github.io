---
layout: post
title:  How NOT to Migrate from Svn to Git
---

OK, where was I?
Oh yeah, I was telling the story about version control in the LANDIS-II community.
I had [described][provenance-lost] how the LANDIS-II source code started on a Subversion server at FLEL, and eventually ended up in a Subversion repository hosted by Google Code in 2011.

[provenance-lost]: /2016/07/21/provenance-lost/

So, last year, in March, Google announced its [plans to shut down][] its project hosting service.
I [relayed this news][] to the L-II developers group, which started a [community discussion][google code going away] about where the L-II code and its provenance should migrate to.

[plans to shut down]: http://google-opensource.blogspot.com/2015/03/farewell-to-google-code.html
[relayed this news]: /email/2015-03-16_JD_1.pdf
[google code going away]: /2016/07/23/google-code-going-away/

Given all the [problems][Not Just Extensions] with the migration <u>to</u> Google Code, I was really concerned how the migration <u>from</u> there would be managed.
I had hoped that the mistakes from the first migration would motivate Rob to better manage this second migration.
Unfortunately, my concerns turned out to be well founded.

[Not Just Extensions]: /2016/07/21/provenance-lost/#not-just-extensions

The developer community put forth a couple options for the new home for L-II’s source code -- CodePlex and GitHub.
SourceForge was also a [viable option][] at that time.[^1]
But it was pretty clear that GitHub was the leading candidate, especially given its [increasing prominence][] in the version control sector.
And that’s where LANDIS-II [now resides][L-II at GH].
That is the clearly the best home for it, even though, as I [pointed out][switch svn to git], this new location required a significant change for our developer community because we had to adapt our workflow and mindset from Subversion’s _centralized_ paradigm of version control to Git’s _distributed_ paradigm.

[increasing prominence]: http://www.wired.com/2015/03/github-conquered-google-microsoft-everyone-else/
[L-II at GH]: https://github.com/LANDIS-II-Foundation
[switch svn to git]: /email/2015-03-29_JD.pdf
[viable option]: https://sourceforge.net/blog/how-to-migrate-from-google-code-to-sourceforge/

So there’s no problem with <u>where</u> our code base was migrated to.
The problems have to do with <u>how</u> it was migrated.


## Insufficient Transparency and Documentation

I expected a more thorough and transparent decision-making process.
But after [my post][switch svn to git] about the implications of switching from Subversion to Git, there was no more discussion for two weeks.
And then Rob simply announced the [decision to migrate to GitHub][GH decision].

[GH decision]: /email/2015-04-12_RS.pdf

It’s not clear how that decision was reached -- how the options were evaluated and compared.
In his announcement, he said there were many reasons, but he didn’t list them.
The rationale behind technical decisions of this magnitude really needs to be documented for the community -- for its current members as well as future ones.[^2]

Furthermore, no specific plans were posted about how the migration would be done.
Rob wrote that _"Google has made such a transition very easy for repository managers"._
But if it was so easy, then how did it get screwed up?
How was a version-control migration poorly managed yet again?


## Failure to use GitHub Importer

GitHub actually provides a tool -- the [GitHub Importer][] -- for importing source code hosted in other version control systems.
This tool can import a single project located inside a multi-project Subversion repository into its own separate Git repository.
Alex and I [demonstrated that it worked][] with 3 different types of LANDIS-II components inside the Subversion repository at Google Code:

* the [Model Core][]
* the LANDIS-II [Software Development Kit][] (SDK) [^3], and
* the [Land Use extension][].[^4]

[GitHub Importer]: https://help.github.com/articles/about-github-importer/
[demonstrated that it worked]: /email/2015-07-15_JD.pdf
[Model Core]: https://github.com/jimm-domingo/model
[Software Development Kit]: https://github.com/jimm-domingo/landis-sdk
[Land Use extension]: https://github.com/jimm-domingo/land-use-ext

The GitHub Importer converts the different entities in a Subversion project into their appropriate Git counterparts:

*  the project's Subversion trunk folder into the Git master branch,
*  its Subversion branch folders into Git branches, and
*  its Subversion tag folders into Git tags.

I can't think of a reason why this tool shouldn't have been used.
Unfortunately, because it wasn’t, the LANDIS-II Git repositories have various problems.

## Crippled Repositories

These problems include:

* Multiple projects shoe-horned into a single Git repository.
  * For example, the [Extensions-Output][] repository contains 10 projects, which Brian M quickly discovered makes it [impossible to use Git branching][BM branch post] on just one of those projects.
* Even Git repositories with just a single project still have Subversion folder structure.
  * For example, the [Century Succession][] and [Biomass Insect][] extensions, as well as the [Biomass Cohort][] library, each have folders called `trunk`, `branches` and `tags`.
* A component's provenance (i.e., its revision history) wasn't imported properly, or not at all.
  * For example, the [Century Succession][] repository only has commits going back to last July when the migration was done.
  None of its revisions in Google Code were imported.
  So, just like the first migration, its [provenance was severed][] once again.
  * At the other end of the spectrum, the [Core-Model][] repository contains two projects: the model core and the SDK.
  Yet it __contains all 3,813 revisions in the entire Google Code repository for all the LANDIS-II components!__
  So its repository contains thousands of empty commits -- commits with 0 insertions and 0 deletions -- that have nothing to do with either project.

[Extensions-Output]: https://github.com/LANDIS-II-Foundation/Extensions-Output
[BM branch post]: https://groups.google.com/d/msg/landis-ii-developers/Wnx6lN1RQ1A/-5FRJpu_pnAJ
[Century Succession]: https://github.com/LANDIS-II-Foundation/Extension-Century-Succession
[Biomass Insect]: https://github.com/LANDIS-II-Foundation/Extension-Biomass-Insect/tree/master/biomass-insects
[Biomass Cohort]: https://github.com/LANDIS-II-Foundation/Library-Biomass-Cohort/tree/master/biomass-cohort-library
[provenance was severed]: /2016/07/21/provenance-lost/#provenance-severed
[Core-Model]: https://github.com/LANDIS-II-Foundation/Core-Model

On the same day that Rob announced the [migration was complete][], Brian M posted about the [problems trying to branch][BM branch post] a single project in the [Extensions-Output][] repository.
[Alex responded][] the following day with a good detailed explanation of the difference between Subversion and Git branching.
He also noted that his colleagues at Notre Dame were also struggling with the switch from Subversion to Git.
So [the issue][switch svn to git] that I had mentioned back at the end of March last year -- the cognitive leap from Subversion's central paradigm to Git’s distributed paradigm -- can be challenging _even for professional developers_ used to the former.

[migration was complete]: https://groups.google.com/d/msg/landis-ii-developers/ORh-eHHJRpI/foLRZuZdTpgJ
[Alex responded]: https://groups.google.com/d/msg/landis-ii-developers/Wnx6lN1RQ1A/ppG5ond0J0YJ

I [followed up][] Alex’s post that same day, describing our success with the GitHub Importer.
I [posted again][] the next day, trying to re-emphasize the critical need to import each LANDIS-II component into its own Git repository.
But my words went unheeded.
In fact, they were not only ignored... they were deleted.

[followed up]: /email/2015-07-15_JD.pdf
[posted again]: /email/2015-07-16_JD.pdf


_Footnotes_

[^1]: I learned _my_ lessons about preserving provenance from the first migration.  I used SourceForge's import service to [archive][] the LANDIS-II project.  That service not only imported all the [revision history][] from Google Code, but also the [wiki pages][] and [issues][] (tickets).
[^2]: That documentation needs to be preserved, not [deleted][google code going away]!
[^3]: Alex has since deleted the SDK repository that he imported from Google Code last year.  So I just re-ran GitHub Importer today using a [URL][SDK at SF] inside the LANDIS-II [archive][] at SourceForge
[^4]: The link points to my personal repository for the Land Use Extension that I imported from Google Code last year.  Alex also imported the extension from Google Code, and then transferred the  new [Land Use Git repository][] to the LANDIS-II Foundation.

[archive]: https://sourceforge.net/projects/landis-ii-archive/
[revision history]: https://sourceforge.net/p/landis-ii-archive/code/3813/log/?path=
[wiki pages]: https://sourceforge.net/p/landis-ii-archive/wiki/browse_pages/
[issues]: https://sourceforge.net/p/landis-ii-archive/tickets/
[SDK at SF]: http://svn.code.sf.net/p/landis-ii-archive/code/sdk
[Land Use Git repository]: https://github.com/LANDIS-II-Foundation/Extension-Land-Use-Change
