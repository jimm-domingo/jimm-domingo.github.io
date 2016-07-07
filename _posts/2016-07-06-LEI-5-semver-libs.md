---
layout: post
title:  LEI 5 - Semantic Versioning for Libraries
---

In [LEI 4][], I recommended a better naming scheme for extensions.
At the end of that post, I wrote that, in this post, I would describe the corresponding naming scheme for extension libraries.
But I’ve realized that I need to present another LEI first before introducing the library naming scheme.
This LEI also has to do with LANDIS-II extension libraries.

[LEI 4]: /2016/07/03/LEI-4-vers-in-ext-names/

Library developers in the L-II community are pretty rigorous about versioning their library code.
They’re continuously striving to clearly communicate changes in their APIs with version numbers so extension developers can adapt and evolve their code accordingly.
Most, if not all, library developers probably already follow the practice described as [Semantic Versioning][].
If they aren’t following the specification exactly, they’re “[probably doing something close to this already][]”.

[Semantic Versioning]: http://semver.org/
[probably doing something close to this already]: http://semver.org/#why-use-semantic-versioning

So the issue is that this community-wide practice isn’t written down anywhere.
It would benefit the whole developer community to simply document a single versioning practice for all extension libraries.
So this LEI is not about establishing a new convention for libraries -- it’s just about codifying current practice.
