---
layout: post
title:  LEI 2 - AsciiDoc for User Guides
---

Last week I had a conference call with a colleague, Alex, about our on-going collaboration.
Our conversation reminded me of one of my ideas for enhancing LANDIS-II.

Like many open-source projects, LANDIS-II records the history of the evolution of its source code  (i.e., code provenance) using version control software (which is currently Git).
The community stores various types of files in [repositories at GitHub][] -- for example: C# source files, Visual Studio project files, sample input files, and user documentation.
All but the last category are text-based, so Git can track the changes to these text files.
And GitHub provides a very convenient way to browse the history of these changes on-line.
So even users can peruse the history of L-II source code with just a web browser.

[repositories at GitHub]:  https://github.com/LANDIS-II-Foundation

Currently, LANDIS-II user guides are stored in Git as Word documents.
These documents are the sources used to generate PDFs, which is the common format for distributing user guides.
The problem is that the binary Word format isn’t compatible with version control software, which is designed for line-oriented text files.

![Diff of a git commit that includes a Word document](/images/diff-with-Word-doc.png)

Through the years, I’ve occasionally wondered if there’s a text-based format we could use instead.

I was reminded of this question when Alex and I were chatting.
We’re collaborating on some documentation for a project outside of LANDIS-II.
Our document was originally in Google Drive, which was certainly adequate for our original use within the project team.
But in preparation to publicly release the document, we ported its contents into [AsciiDoc][] in a GitHub repository.
This conversion was an opportunity -- a pilot project of sorts -- to explore this lightweight text-based markup language and some associated tools:

* [Atom][], an open-source text editor, and
* [AsciiDoc Preview][], an optional package for Atom.

[AsciiDoc]:  http://asciidoctor.org/docs/what-is-asciidoc/
[Atom]:  https://atom.io/
[AsciiDoc Preview]:  https://atom.io/packages/asciidoc-preview

This screenshot of Atom shows our AsciiDoc file on the left (with the `.adoc` file extension), and the preview of our file rendered in HTML on the right.

![Screenshot of AsciiDoc file in Atom text editor with AsciiDoc Preview package](/images/AsciiDoc-Atom.png)


Because an AsciiDoc file is line-oriented, and the recommend practice is [one sentence per line][], changes to a file are easy to view at GitHub:

[one sentence per line]:  http://asciidoctor.org/docs/asciidoc-recommended-practices/#one-sentence-per-line

![Diff of a git commit with AsciiDoc file](/images/AsciiDoc-diff.png)


Alex and I agreed that it was a successful pilot project for this documentation format.

Based on our experience, I feel AsciiDoc warrants consideration by the L-II community.
The next step would be to find a developer team for an extension who would volunteer to convert their user guide from Word to AsciiDoc.
They can use the AsciiDoc Preview package to export a self-contained HTML file for their guide, which can be distributed with their extension.
Their experience with the conversion would provide further information to help the community evaluate AsciiDoc.
