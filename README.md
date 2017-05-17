# submodule-test-super
Sample git repo that contains a submodule. This is the "super" repo, following the example in Fraser Speirs'
[Understanding Git Submodules](http://www.speirs.org/blog/2009/5/11/understanding-git-submodules.html) article, as excerpted below:

> This is a repository which contains a Git repository called 'a" and another called "super". We will add "a" as a submodule of "super":
>
>~~~~
>[/tmp/git]$ ls -l
>total 0
>drwxr-xr-x  4 fspeirs  wheel  136 May 11 11:03 a
>drwxr-xr-x  4 fspeirs  wheel  136 May 11 11:03 super
>~~~~
>~~~~
> The first thing to do is run "git submodule add" in super:
>~~~~
>[/tmp/git/super(master)]$ git submodule add /tmp/git/a ProjectA
>Initialized empty Git repository in /private/tmp/git/super/ProjectA/.git/
>
>[/tmp/git/super(master)]$ git submodule status
>-85ab8ba4edf9168ab051ded7ddbbe20861b71528 ProjectA
>
>[/tmp/git/super(master)]$ ls ProjectA/
>a.txt
>~~~~
>Having done that, let's look at the impact of that command on the project "super":
>~~~~
>[/tmp/git/super(master)]$ git status
># On branch master
># Changes to be committed:
>#   (use "git reset HEAD ..." to unstage)
>#
>#	new file:   .gitmodules
>#	new file:   ProjectA
>#
>~~~~
>We have the new .gitmodules file, which should be checked in, and a new file called "ProjectA", which is the "path" of our submodule. Let's commit these two now:
>~~~~
>[/tmp/git/super(master)]$ git commit -m "added submodule"
>[master]: created ffba648: "added submodule"
> 2 files changed, 4 insertions(+), 0 deletions(-)
> create mode 100644 .gitmodules
> create mode 160000 ProjectA
>~~~~
>Note the mode "160000" on ProjectA - that's a special mode for a certain kind of entry in the Git index. It's different from normal files.
>
>Now, if we look at the contents of the Git index, we'll see the SHA-1 for the tracked files:
>~~~~
>[/tmp/git/super(master)]$ git ls-files --stage 
>100644 831cdc0dc1b88e69aa9943cf09907ae1bcd031fc 0	.gitmodules
>160000 85ab8ba4edf9168ab051ded7ddbbe20861b71528 0	ProjectA
>100644 16f5c2d3aa9656fc424352e4cfaa2523c809778b 0	super.txt
>~~~~
>Notice the SHA for ProjectA - 85ab8ba - this is the SHA-1 of the commit to which the submodule is locked in Project A. Commit 85ab8ba does not exist in the "super" repository - it refers to a commit in a submodule repository.
>
>So Git now knows the three things required to set up your submodules when cloning a project:
>
>- The what comes from the "URL" property in the submodule's entry in your .gitmodules file.
>- The where comes from the corresponding "Path" entry in .gitmodules.
>- The when, if you will, comes from the SHA-1 stored in the superproject's index file for the remote.
