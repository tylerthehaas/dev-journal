# November 26th, 2020

## what does `git rev-parse --abbrev-ref HEAD` do?

### What is a revision?

A revision is an object that has an id, descriptor some other data and the changes that were made stored in a directory and some files within .git

### rev-parse

in [gits docs](https://git-scm.com/docs/git-rev-parse) it says:

> git-rev-parse - Pick out and massage parameters

its a plumbing command that can be used to parse the revision object and pick out certain parts to suit your purposes.

### --abbrev-ref

A revision object aka a commit hash can have a reference or ref for short that is more human readable. An example of this are git tags and branches. So when we run rev-parse with the abbrev-ref flag we are asking git to parse the revision object and give us a short human readable reference name back which usually maps to a branch name.

#git
