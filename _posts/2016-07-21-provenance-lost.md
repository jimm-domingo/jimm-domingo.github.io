---
layout: post
title:  Provenance Lost
---

> If you have never used version control before, the first thing you should do is go find someone who has, and get them to join your project.
> These days, everyone will expect at least your project's source code to be under version control, and probably will not take the project seriously if it doesn't use version control with at least minimal competence.
>
> -- Karl Fogel, [Version Control][], [Producing Open Source Software][]

[Version Control]: http://producingoss.com/en/vc.html
[Producing Open Source Software]: http://producingoss.com/en/

When I joined the Forest Landscape Ecology Lab ([FLEL][]) in 2004, the LANDIS-II project wasn’t using version control (VC), so I setup a Subversion server in our lab.
As I later recalled in [this post][] in the developers' Google group, we used Subversion because it was mature, and because distributed VC systems like Git weren’t available back then.

[FLEL]: /about/#flel
[this post]: https://groups.google.com/d/msg/landis-ii-developers/WPFM0h8Ts4g/LZQxJvH8CoYJ

We used our own Subversion server because we were in the early stages of development.
Therefore, we needed to limit access to the code until initial research with LANDIS-II was published.
After that [publication][Scheller et al 2007], we opened access.

[Scheller et al 2007]: http://dx.doi.org/10.1016/j.ecolmodel.2006.10.009

I maintained our server while I worked at FLEL.
When I left the lab in 2007 to continue working on L-II as a freelancer, the responsibility for the server shifted to other lab staff.
_(A big thank-you to Ted, Sarah and Jodi for doing that over the years without complaint.)_

The system didn’t require much attention initially because it was fairly stable.
Once in a while, Apache or Windows (usually the latter) got its knickers in a twist, so they would have to kick the box (i.e., restart it).
But gradually, the system needed kicking more frequently.
This increasing lack of uptime and reliability was the big motivation to move the L-II source code and its revision history -- the __code's provenance__ -- to an external Subversion service in 2011.

Fortunately, several years earlier, Google had started its free [hosting service][] for open-source projects.
So LANDIS-II's source code was migrated from FLEL's Subversion server to [Google Code][landis-extensions].
Rob was responsible for that, but he chose not to ask for my help.
Therefore, the migration to Google Code wasn't done competently, and unfortunately, it introduced several problems into our community.

[hosting service]: https://developers.googleblog.com/2006/08/project-hosting-us.html
[landis-extensions]: https://code.google.com/archive/p/landis-extensions/


## Not Just Extensions

First, the code was migrated into a project called “[landis-extensions][]” instead of “landis-ii”.
Why?
The explanation Rob told me later on was that David was concerned about releasing the L-II core on Google at that time.
I couldn’t fathom why not.
I would’ve gladly explained to David what I told Rob -- there’s nothing special or novel in the core that would warrant that concern.
Take a look at [its code][core src] for yourself -- there’s nothing in the core modules that would have given an unfair advantage to another research group.

[core src]: https://sourceforge.net/p/landis-ii-archive/code/HEAD/tree/model/trunk/core/src/

Anyhow, Rob’s initial plan was to put just the extensions into that Google Code project.
That’s why it’s named “landis-extensions” and not “landis-ii”.
Unfortunately, that name would confuse new developers when they joined our community.
They would naturally assume (_incorrectly_) that other L-II components -- the model core, the console and its debugging counterpart, the extension manager, libraries, etc -- were located elsewhere.
They’d eventually realize that those components actually resided in “landis-extensions” too.

## 2 Trunks are not Better than 1

The project name wasn’t the only source of cognitive friction.
Early on with its code-hosting service, Google assumed a Subversion repository would contain a single project.
Therefore they automatically created the standard Subversion directory structure for a single-project repository:

    trunk/
    branches/
    tags/

You can see it here in the [first revision][].
Note that __revision 1 has no author__, so only Google’s infrastructure could have created that revision.
All of us L-II developers had to sign in to commit [new revisions][archive-log].

[first revision]:  https://sourceforge.net/p/landis-ii-archive/code/1/
[archive-log]:  https://sourceforge.net/p/landis-ii-archive/code/3813/log/

But was this inappropriate structure fixed?
Was it corrected to match the standard multiple-project layout that was already on our Subversion server?

No.

Rob just started creating project subfolders inside the top-level `trunk/` folder.
So, as a result, an extension's active code development occurred in __a folder whose path had 2 `trunks` in it!__
For example, here's the path to the `trunk` folder for the Century Succession extension:

*  [/trunk/Century-succession/trunk/][century-succ trunk]

[century-succ trunk]: https://sourceforge.net/p/landis-ii-archive/code/HEAD/tree/trunk/Century-succession/trunk/

This was another source of confusion -- of cognitive friction -- for new developers.
Many of them aren’t professional developers -- they’re grad-students, post-docs and researchers -- so they were learning about version control and Subversion.
They certainly didn’t see any URLs in Subversion documentation with `trunk` appearing twice.

It was even more problematic with URLs for branches and tags.
It’s really challenging to explain branching to a new L-II developer with this distorted folder structure:

*  [/__trunk__/Century-succession/__branches__/Biomass Library/][century-succ branch]

[century-succ branch]: https://sourceforge.net/p/landis-ii-archive/code/HEAD/tree/trunk/Century-succession/branches/Biomass%20Library/

_“Yes, you have the correct URL for that branch even though it has `trunk` in its path.”_

## Provenance Severed

The third, and by far the most crucial problem with the migration is that Rob did not dump the revision history from our Subversion server.
Google provided the means to import a repository dump file, so a professional developer would have done that to ensure that the project’s revision history was migrated.

Instead, he just committed the current copy of the code.
So his failure to dump and import the history resulted in __fragmenting the code’s provenance__.
If a researcher wanted to peruse the history of changes to an extension’s source code, she could only go back to early 2011 in the Google Code repository.
__The first 7 years of provenance had been severed__, and therefore, had to be viewed elsewhere.

In the interim, it could be viewed on our Subversion server at FLEL.
But that wasn’t a long term option given the system’s increasing unreliability.
Furthermore, FLEL wanted to repurpose the hardware.

So when I was in Madison in January 2012 for the LANDIS-II workshop, I went to the lab to retrieve the code’s revision history from our Subversion server.
I distinctly remember Rob stopping by the lab, because it was the first time we saw each other at the workshop.
After exchanging “Hello”s, I explained what I was doing about dumping the repository's history.
He replied that it wasn’t important now that code was at Google.
I disagreed, and proceeded anyways (_this wasn’t the first time we had a hugh difference of opinion, nor the last_).

I uploaded the archives to the web hosting provider, [WebFaction][], that we were using at the time for the L-II web site.
I set up [WebSVN][] -- software for browsing Subversion repositories -- to serve up the archives online.
I [notified][] several key people, including Rob and David, about the archives.
So even though the first 7 years of code history had been severed from the on-going history, at least that earlier history was available online as a reference for the L-II community.

[WebFaction]: https://www.webfaction.com/
[WebSVN]: http://www.websvn.info/
[notified]:  /email/2012-01-27_JD.pdf

## Provenance Lost

Sadly, that resource didn't survive long.

In the summer of 2013, Rob told me about his intent to [migrate the L-II web site][] from WebFaction to Google Sites.
It seemed like a good idea at the time, and to this day, I still think it was.
We traded a [couple more emails][] where I asked when our pre-paid balance at WebFaction would run out.
He didn't answer my question, but did announce that he had already created the new site.
I sent another email about [trouble accessing the new site][], but never got a response.

Rob didn't inform me or the community about a specific timeline to close down our WebFaction account.
I assumed when he did, that he would retrieve all the files that had been uploaded there.
Or at least, ask me if there was anything.
If he had, I would definitely stressed the importance of the retrieving the code archives.

[migrate the L-II web site]: /email/2013-08-14_RMS.pdf
[couple more emails]: /email/2013-08-26_RMS.pdf
[trouble accessing the new site]: /email/2013-09-01_JD.pdf

I would only discover months later, in May 2014, when I inquired about [other files][Plone notes] that I had stored in the account, that he hadn’t retrieved the code archives.
He [hadn't used the appropriate software to access the files][did not use ftp] in our account.
I contacted support at WebFaction, but alas, they don’t retain back-ups of closed accounts (which makes sense for privacy and security reasons).

[Plone notes]: /email/2014-05-30_RMS_1.pdf
[did not use ftp]: /email/2014-05-30_RMS_2.pdf

So in 2013, __the first 7 years of code provenance__ which had been severed __was permanently lost.__
Collectively, Rob and I were responsible for curating that critical technical asset.
But we failed to preserve a significant part of the community’s history.
To this day, I’m still very upset about that loss, which was clearly avoidable.
And I’m very sorry for my part in it.
