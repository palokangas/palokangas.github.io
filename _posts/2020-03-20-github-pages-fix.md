---
layout: post
title:  "Github pages fix for duplicate installation with Octopress"
date:   2020-03-20 16:41:41 +0200
categories: programming
tags: [github, blog]
---

I just spent too much time to fix a weird problem in my fresh github pages site. I just could not make the /blog-subdirectory work. It produced a weird old Octopress template page even though I am using Jekyll.

After trying everything and concluding that this is somebody else's bug (yeah, github rather than me) and renaming my blog to "posts" it finally dawned: check my old repos.

And yes, it turns out I had an old empty private (!) repository named "blog" from 2011. And this was somehow messing up with the /blog-subdomain. Cheez that was both simple and hard!
