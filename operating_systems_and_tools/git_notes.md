Speaker has a love/hate relationship with git
  - hg's user interface makes more sense than git's
  - git's commands makes assumptions about your workflow
  - however git's data model works better than hg's.

In git, a branch is just a pointer. That's all it is.

You can completely change what a branch is by making it point somewhere else.    
You do this by doing `git reset --hard $commit_ref`, where $commit_ref can be one of the following:
  - hash
  - tagname
  - branchname
    - If you have a branch "foo" and a branch "bar", you can make foo point at wherever bar is at
      by doing `git reset --hard bar`.
    - HEAD is a virtual branch name which refers to the current latest commit on your current
      latest branch
      - And if you put a ~ after it, you get its parent, ~2 you get its grandparent, and so on.
        - this is why `git reset --hard HEAD~` is used to undo committed changes, because it goes
          to the parent of the current commit

You're juggling pointers to different revisions, and if you drop it, it can be difficult to pick it
up again. 
  - In the above example, if you move a branch pointer that was on a single commit that no other
    branch had, that commit will be "orphaned". Then it becomes difficult to find it again because
    no branch refers to that commit in its history.
  - You would use `git reflog` to find those again, but "you should always have the man page open
    when using it".

The staging area is like a virtual "next commit" that we're adding or removing things to.
  - Since a commit is essentially a snapshot of all the files in your repo, this is why if you
    change a file after you `git add` it, it's both in the staging area in its unchanged form and
    unstaged with the new changes.

`git gui` is a way to choose which changes you want to add.
  - You can choose to stage changes by "hunk" (a block of changes), linewise, and so on.
  - You can do the same with `git add --interactive`, but a GUI makes stuff a bit easier.

Bisecting
  - It worked 3 months ago, it doesn't work now. `git bisect` good point is 3 months ago, bad point
    is now -> bisects the history to get you something from 6 weeks ago (halfway through) and asks
    you if it works or not.  Repeats this over and over again (binary search) to find the commit
    that broke the thing.

`git rebase $commit_ref` re-commits everything it touches, giving it new hashes - even if those
commits were not changed at all. So you should try to only go back as far as necessary.

No. 1 piece of advice Robert can give on Git:  
`git config --global merge.conflictstyle diff3`
  - This is a different version of the merge conflict message that gives more information,
    which is better. So do some research and use it.

`git fetch $upstream $branch` is better than `git pull`, because `pull` automatically
tries to merge, which can suck...

You can refer to a *range* of commits too by using two different commit
references and the --onto syntax for rebase.


