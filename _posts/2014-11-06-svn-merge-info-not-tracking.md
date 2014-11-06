---
author: Joe Kampschmidt
layout: post
slug: svn-merge-info-not-tracking
title: Why is SVN Merge Tracking not working?
desc: 'Brief explanation why SVN and TortiseSVN might not be recording the revision in the Merge Info properties'
tags:
- svn
---

The most likely answer:

**The URL to merge from is case-sensitive. If you merge from a URL with the wrong casing the merge will work for the files but the revision will not be recorded in the MergeInfo property.**

This can be a frustrating problem because SVN and [TortiseSVN][0] do not give any feedback/notification about the mis-matching urls during a [merge][2].

## How I figured it out

Recently, my svn client merge-tracking stopped working. I was performing merges with [TortiseSVN][0] at the root level but the revision numbers where not being recorded in the MergeInfo Property. There were no error messages either.

I then tried merging directly with svn from the command line but got the same result. No error messages either.

It was not until I thought to try to do a merge from the command line with the `--record-only` option. This was the only command that provided an actual error message:

	svn merge -c 16304 https://server/svn/project/trunk/ --record-only
	svn: E200004: Merge from foreign repository is not compatible with mergeinfo modification

Once I had a meaningful error, I was able to track down this old forum conversation (argument) about [case sensitive urls and TortiseSVN][1].

I hope this saves you some time.

[0]: http://tortoisesvn.net/
[1]: http://tigris-scm.10930.n7.nabble.com/Why-Merge-Tracking-is-disabled-td24815.html
[2]: http://svnbook.red-bean.com/en/1.7/svn.branchmerge.basicmerging.html
