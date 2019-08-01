# Understanding Branches

## Learning Goals

- Define a Git branch
- Identify a local branch
- Identify remote branches
- Identify remote tracking branches
- Identify local tracking branches

## Introduction

We're faced with many choices in life. Some of these choices are easier, more
convenient or "safer" than others.

For example, think about upgrading a computer. Imagine we want to add a new,
bigger hard drive to our computer. However, we've never attempted tinkering with
computer hardware before. And our only teacher is some video on YouTube. Also,
we really don't want to break our computer because it's expensive to replace and
it's got all our favorite cat videos on it.

We might choose to "do nothing" because the risk is so great. Because of the
risk, we might not want to try new things. Our freedom to try new things is being
held back by fear.

The same can be true in code: if we somehow barely get our code working, we
might be afraid to go as far as we ought in order to create clean, maintainable,
understandable code because we're afraid we'll break it.

Git lets you experiment on uncertain code solutions until it works and then
attach that working code to your past working code. If your experiment blows up,
you can simply throw it away without damaging your working code. Git lets us go
from working code to working code, from success to success. We create "parallel
universes" for safe experimentation which can be integrated ("merged") or thrown
away using branches.

## Define a Git Branch

A _branch_ is a history of the commits, or "checkins" or "savepoints" pointed
to by a name, called the "branch name."

![The relationship between parent and child branches](https://curriculum-content.s3.amazonaws.com/programming-univbasics-2/Image_2_Parent_Child%20Branches.png)

In this image, the main branch is called `master`. In fact the default, main,
most-important  branch in a Git repository is `master`.

> **IMPORTANT**: The default "main" branch in a Git repository is called
> `master`

Branching allows us to take all previous commits with us to a parallel universe
(or sandbox, or safe zone). In the image above, the `little-feature` branch
inherits the first blue commit and then adds its own purple commit. The
`little-feature` branch never comes back to to `master`. Maybe that idea didn't
work out. Or maybe the developer went on vacation and will get back to it
later. We don't know, but the work in that branch didn't impact this team's
ability to add two more commits.

We also see in the image that the second commit of the `master` branch never
reaches `little-feature`. They're in separate "universes." It's worth
repeating, whatever changes are in `little-feature` ***have no impact*** on
`master` or vice-versa.

The teal branch `big-feature` has the first three commits of `master` and adds
three commits of its own. Again, changes made in `little-feature` are neither
in `master` nor in `big-feature`.

The branch we start working from is often called the "parent" branch. The
"split-off" branch is often called the "child" branch. In the image, the parent
branch is `master` and the child branches are `little-feature` and
`big-feature`.

Immediately after branching the child from the parent, the commit histories
are identical. They run the same code the same way because they have the same
history. We often say those "branches are equal."

![The relationship between parent and child branches](https://curriculum-content.s3.amazonaws.com/programming-univbasics-2/Image_2_Parent_Child%20Branches.png)

As commits are made onto the child branch, we say it is "ahead" of its
"parent." The "parent" is said to be "behind" in terms of commits. When we
merge the commits unique to the "child" branch to the "parent" (as we'll see
later), the two become "equal" again. This is how we create the effect of
"moving from success to success" that we described earlier.

Now let's clarify some terms around different types of branches and
related commands before we jump into working with them.

## Identify a Local Branch

A **local branch** is a branch that only we (the local user) can see. It exists
only on _our_ computer. The `git branch` command can show us our local
branches:

```bash
$ git branch
  * master
  larry-branch
  michelle-branch
  curly-branch
```

## Identify Remote Branches

A **remote branch** is a branch on a (remote) repository stored on a server or
a code hosting service like [GitHub](https://github.com).

We can copy the whole repository using `git clone`. Once we have our own copy
of the repository, we can _push_ new local branches to it, fetch updates from
remote branches to our local repository, or fetch new branches from the remote
repository.

![Illustration of a remote branch](https://curriculum-content.s3.amazonaws.com/git-workflow/Image_4_Remote%20Branches.png)

By default, when we `git clone` a repository our local instance of Git gives
that repository a human-friendly name called the "shortname." The default
"shortname" is called `origin`. Many Git commands assume that if you don't
specify a repository explicitly, you mean `origin`.

We can see a list of all the remotes' "shortnames" with `git remote`. Usually,
there is only one, `origin`, the default. (As you get more skilled, you might
want to share your code to multiple remotes.)

We can see a list of all ***remote branches*** &mdash; that is all the branches
at all the remotes &mdash; that Git knows about with `git branch -a`.

The output will include a number of branch names that start with `remotes/`. As
you might have guessed, those are the remote branch names. `remotes/` signifies
that it's remote; `origin/` signifies this branch is held on the `origin`
repository` and, after that last `/`, we see the remote branch name.

Here's an example (your output may vary slightly):

```shell
...
remotes/origin/HEAD
remotes/origin/master
remotes/origin/wip-add-graphics
```

## Identify Remote Tracking Branches

A _remote tracking branch_ is a _local_ copy of a _remote_ branch.

When a locally-created branch like `new-branch-name` is pushed to `origin`
using the `git push -u origin new-branch-name` command, a remote tracking
branch named `remotes/origin/new-branch-name` is created on your machine. This
makes sense, the branch "lives" somewhere else "remote" in the "origin"
repository whose remote branch name for this branch is `new-branch-name`

On the reverse side, when you clone a repository, you're given remote tracking
branches for _all_ the remote branches. Just as in the previous paragraph, the
remote tracking branches are of the form `remotes/origin/new-branch-name`.

This remote tracking branch "follows" or "tracks" the remote branch
`new-branch-name` on `origin`. We can update your remote tracking branch to be
in sync, or up-to-date, with the remote branch using `git fetch`. If you either
`git clone` or refresh your initial cloned information  with `git fetch` right
before you go into the wilderness, you can have _years_ of Git history on your
laptop.

## Identify Local Tracking Branches

A _local tracking branch_ is a local branch that is tracking another local
branch. Most often, the local branch is tracking a remote tracking branch
(that, in turn, is tracking a remote branch).

![Illustration of a local branch](https://curriculum-content.s3.amazonaws.com/git-workflow/Image_3_Local%20Branches.png)

This sounds confusing and this is an area that can be really frustrating with
Git. Don't worry! We'll work through it with you.

Let's suppose you and a collaborator are having lunch and you mention that you
wanted to start some work in `master`, but that you weren't sure how to start
because the code is so messy (that happens!). The collaborator has great news:
they just updated that messy code so that it's a _joy_ to work with.

In this case, you'd like to start working from _their_ branch _instead of_ the
default, `master`. Your collaborator pushes up their local branch to the
repository under the name `my-friends-branch`.

You go back to your computer and then type `git fetch`. This tells your clone
of the repo to refresh information in the remote repository. Git will tell you
that there's now a remote branch available called...`origin/my-friends-branch`.

If you do `git checkout origin/my-friends-branch`, Git will:

1. Establish a remote tracking branch called (`remotes/origin/my-friends-branch`) that points to `origin/my-friends-branch`
2. Establish a local tracking branch (`my-friends-branch`) that tracks (`remotes/origin/my-friends-branch`)
3. Put you "on" the local tracking branch (`my-friends-branch`)

Because Git can trace how to get from your local tracking branch to the remote
tracking branch _back_ to the original remote branch, if you make a commit on
this branch and then type `git push`, Git will send your change to the remote
repository so that other collaborators can get your change and build on it!

This is the heart of collaboration in Git. Your collaborator found a problem,
created a branch and fixed it, and that enabled you to extend your
collaborator's idea. You both contributed to making the code better. In turn,
the branch `my-friends-branch` can be merged to `master` and you two will be
celebrated as heroes for making `master` a measurable amount better.

## Conclusion

With branches, Git helps us take risks with our code and gives us the freedom to
experiment. We will work regularly with local and remote branches, as well as
local and remote tracking branches. Next, we'll take a look at how branches look
in practice.

## Resources

- [Git Branch](https://www.atlassian.com/git/tutorials/using-branches)
- [3.1 Git Branching - Branches in a Nutshell](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
- [Using branches](https://backlog.com/git-tutorial/using-branches/)
