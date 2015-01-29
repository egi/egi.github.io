---
layout: post
title: "Loosing Access to SVN Trunk"
description: "What to do when suddenly you only have access to one SVN branch while previously have access to SVN all trunk and branches?"
headline: "What to do when suddenly you only have access to one SVN branch while previously have access to SVN all trunk and branches?"
modified: 2015-01-28

comments: true
share: true
---

If your main repo is in SVN, and you use GIT as your local development tool, a clean way is to commit to SVN directly, and hold Git update until you get back your full SVN access.

~~~ bash
$ cd git-repo
$ git diff --no-prefix from to > patch-file-from-git.patch
$ cd svn-repo
$ svn checkout branch-url
$ patch -p0 < patch-file-from-git.patch
$ svn add
$ svn commit
~~~

It is hard (if not impossible) to update your Git SVN state, without trunk access. Even if you are able to [cherrypick the branch](http://stackoverflow.com/a/10173516) that you fetch.

Some of the options to explore are based on logic that `git pull` == `git fetch && get merge`. So when we were unable to pull, we can fetch svn manually for the tracked branch, and make git <del>merge</del> rebase to it. Remember, this is git-svn. Merge can [fubar the git repo](http://stackoverflow.com/a/4168434).

- create [separate svn-remote](http://stackoverflow.com/a/1430782) section for the specific branch.
  then you can fetch directly to the branch without fetching the trunk.

~~~
$ git config --add svn-remote.newbranch.url https://svn/path_to_newbranch/
$ git config --add svn-remote.newbranch.fetch :refs/remotes/newbranch
$ git svn fetch newbranch [-r<rev>:HEAD]
$ git checkout -b local-newbranch -t newbranch
$ git svn rebase newbranch
~~~

- `git svn rebase` equals to `git svn fetch && git rebase trunk master`
   this explains why svn trunk access is always needed.
- rebase manually? `git svn rebase -l`


## ref:

- http://git-scm.com/book/en/v2/Git-Branching-Remote-Branches
- http://stackoverflow.com/questions/296975/how-do-i-tell-git-svn-about-a-remote-branch-created-after-i-fetched-the-repo
- http://durdn.com/blog/2011/07/06/using-git-with-subversion-tips-that-will-make-your-life-easier/
- http://stackoverflow.com/questions/10606535/git-svn-rebase-vs-git-rebase-trunk
- http://stackoverflow.com/questions/4168411/how-does-git-svn-know-which-branch-to-dcommit-to
