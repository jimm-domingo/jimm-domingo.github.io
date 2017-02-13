---
layout: post
title:  Learning Lessons While Building L-II
---

I hadn't planned on taking time away from this blog after my [last post][] about being censored in the LANDIS-II developer community.
Yet the break has been an opportunity to rejuvenate, because, quite frankly, revisiting that history isn’t pleasant.
<u>Dwelling</u> on the past isn’t helpful.
But <u>studying</u> the past is, if one can uncover the lessons it has to offer.

[last post]: /2016/07/27/landis-ii-censorship/

So the break has also been an opportunity to reflect.
I’ve been working to find the lessons that the experience can teach me.
I know from my history with LANDIS-II that are lessons in there somewhere.
Lessons that are worth looking for because the practice of identifying and incorporating such lessons was crucial to our success in building L-II in the first place.

## Iteration Assessments in the Unified Process

We developed the first version of LANDIS-II -- version 5.0 [^1] -- using the Unified Software Development Process, or the [Unified Process][] for short.
The L-II leadership (David, Eric, Brian S.) had already selected this methodology before I joined the project.
Since I hadn’t used this process before, I studied up on it.
My professional library still has several Unified Process books including, of course, _the_ book -- the one written by the 3 leading software methodologists who integrated their work to create the Unified Process:

<p align="center">
<img src="/images/USDP-book-front-cover.jpeg">
</p>

[Unified Process]: https://en.wikipedia.org/wiki/Unified_Process

They identify the real distinguishing aspects of the Unified Process as:

*  Use-case driven
*  Architecture-centric
*  Iterative and incremental

Iterative and incremental development is certainly not unique to the Unified Process.
It’s hard to imagine any software project succeeding without an iterative process.
One facet of iterative development that the creators of the Unified Process emphasize is "iteration assessments"[^2]:

> If the benefits of the iterative way of working are to be realized fully, the project must assess what it has accomplished at the end of each iteration and phase.  The project manager is responsible for this assessment.  It is done not only to assess the iteration itself but to promote two further goals:
> * To replan the next iteration in light of what the team has learned in doing this one and to make any necessary changes.
> * To modify the process, adapt the tools, extend training, and take other steps as suggested by the experience of the assessed iteration.
>
> -- Section 12.8, [The Unified Software Development Process][]

[The Unified Software Development Process]: https://books.google.com/books/about/The_Unified_Software_Development_Process.html?id=s1OdpwAACAAJ


## Lessons Learned With L-II 5.0

As the project manager for LANDIS-II 5.0, Rob deserves full credit for reliably fulfilling this assessment responsibility throughout our whole development cycle.
Even as challenges arose and filled our plates, he ensured that we assessed every iteration.
Those [assessments][] are still available on-line on the [LANDIS-II Developers][] web page.
At end of each iteration, we identified the lessons learned during that iteration, and how we could modify our process to improve it.

[assessments]: /docs/History_of_LANDIS-II_Development_2004-2005.pdf
[LANDIS-II Developers]: http://www.landis-ii.org/developers
For example, we introduced time estimates for tasks in iteration #8 because of schedule slippage.
As we completed more and more components of LANDIS-II and the size of its code base increased, so did the number of “Misses” in our iteration assessments.
Part of the reason for this increase is that we were including a couple “bonus” tasks in our iteration plans to avoid down time in case we caught a tailwind during an iteration and completed all the tasks early.
With time-boxed iterations, was it better to have too few tasks per iteration or too many? [^3]
We favored the latter.

At the end of iteration #8, we realized that the time estimates weren’t worth the effort to develop them.
So we replaced them with rankings, marking the bonus or secondary tasks as “time permitting” or “low” priority in iterations 9 and on.
The rankings helped us focus on the primary tasks for an iteration.
If one or more of these tasks took longer than expected, we could table secondary tasks for later.
Any postponed tasks would then become primary tasks in the next iteration.
Iteration 9 and its successors confirmed that this was a better practice for our team.

Lessons like those above definitely improved our software development process.
Our process wasn’t ever perfect -- any endeavor involving a group of people will inevitably encounter difficulties along the way and occasionally stumble.
But I strongly believe that our use of the Unified Process with its practice of assessing not only _what_ we were building but also _how_ we were doing it, provided many valuable lessons about both facets of our project.
Using those lessons to refine our process was key to reaching our goal: the construction of LANDIS-II.


_Footnotes_

[^1]: I’ll explain why we started with version 5 in a future post.
[^2]: Sometimes known as [iteration retrospectives][] in the Agile community.
[^3]: In hindsight, it’s easy to understand the rationale for [backlogs][] in Agile methodologies.

[iteration retrospectives]: https://www.agilealliance.org/glossary/heartbeatretro/
[backlogs]: https://www.agilealliance.org/glossary/backlog/
