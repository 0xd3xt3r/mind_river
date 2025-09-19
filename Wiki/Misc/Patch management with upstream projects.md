---
aliases:
  - patch management
  - patch
  - upstream projects
tags:
  - open-source
  - dev
---


## Terminology

1. Upstream - the developer who writes the source code
2. downstream - person who maintains the binary package for some distro
3. out-of-tree code - 
4. backport - applying patch on the older release/version of the project

- Examples
	- Let say, new version of glibc is released just before a new version of Fedora, so Fedora can get the latest version of glibc. Fedora is thus "downstream" of the main glibc project, and glibc is "upstream" of Fedora. Since RHEL is based on Fedora, RHEL is even further "downstream" of glibc.
## Commands

```bash

## Patch creation options

# where `<N>` is number of last commits to save as patches.
git format-patch HEAD~<N>

# To generate patch if you haven't commited the changes
git diff > mypatch.patch

# If you new files that are untracked and won't be in your `git diff` output.
# stage everything for a new commit `git add` the file you want and then
git diff --cached > mypatch.patch

# patch from a single commit
git format-patch -1 <commit SHA> -o <name of the directory you want the patch saved to>

# patch from a branch
git format-patch <branch name> -o <name of the directory you want the patch saved to>

## Apply a patch

# takes a patch (e.g. the output of git diff) and applies it to the working directory (or index, if --index or --cached is used).
git apply 

# First the stats
git apply --stat a_file.patch

# Then a dry run to detect errors:
git apply --check a_file.patch

# To apply the patch as a 'commit'
# apply patch created using 'format-patch'
git am

```

## Workflow

Checklist:
- Make sure your patch is complete and correct, including formatting.
- Make sure your patch includes relevant documentation.
- Make sure your patch is sufficiently tested.

This implies some rules about naming your patches (the subject in your emails). Patchwork can combine a multi-patch series into a single unit, or replace obsolete versions of your patch with a new version, if you and it agree on some conventions. As usual, these are all detailed in the checklist, but it boils down to a subject line like this (which you get by default with a tool like git-send-email):

*[PATCH v1 1/4] malloc: fix a bug [BZ #123456]*

This lets patchwork know where in the series it is (1/4, 2/4, etc) and which version it is (v1, v2, etc; increment this each time you update a patch, and remove any "Re:" from your subject line when you do, for we all know that "Re:" is short for "Reply:" and if you say it's a reply, it's a reply). 

If you're fixing a bugzilla, include that in the subject too. Patchwork additionally keeps track of the state of the patches, like who is assigned to review them, or which checks have passed...

## Ref

1. https://www.redhat.com/en/blog/patches-welcome-how-contribute-upstream-glibc
2. [glibc contribution checklist](https://sourceware.org/glibc/wiki/Contribution%20checklist)
3. [Patch workflow](https://sourceware.org/glibc/wiki/Patch%20Review%20Workflow)
