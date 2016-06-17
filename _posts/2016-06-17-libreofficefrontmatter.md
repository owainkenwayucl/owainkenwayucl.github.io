---
layout: post
title: Libreoffice test
---

I tried editing posts in Libreoffice (and saving as plain text) so that I could use the spell checker and it broke the YAML front matter.

Looking at the git diff, it reproducibly inserts some control characters at the start of the document:

![Screenshot of git diff](/images/librefrontmatter.png)

These characters are non-printing and therefore you have to get your editor to display them before you can remove them.

This is quite an annoying feature!
