# Diff Graphs in `git`
`git` is a wonderful beautiful piece of software, used by millions of professionals around the world; but far too many of us only know how to operate it through arcane rituals and rote memorization, lacking instruction and insight on its inner workings.

This article aims to give you a working framework of knowledge concerning `git`. Comparisons between `git` and other VCS tools is out of scope for this article: those are straightforward enough to find, and if you're here you're probably already sold on the merits of `git` through osmosis if nothing else. For the sake of inclusivity, this article doesn't assume any other previous knowledge or experience using `git`.

If you're still curious to establish or expand your understanding of the way this beautiful tool works for you, read on, read on.

## Baby's First Commit
Everything `git` does and cares about lives inside a repository. We'll elaborate on what *repositories* -- or colloquially *repos* -- are, and what they contain, in a moment. For now, let's create a fresh local repo on your machine. Start by [installing `git`](https://git-scm.com/book/en/v2/Getting-Started-Installing-`git`) and performing the [first-time setup](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), if you haven't already.

### Making a repo
There are a few ways to act on `git` repos: some people prefer using a desktop app with a GUI that lets them run predefined actions in repos; but for greater control and deeper understanding, we'll use our terminal and the `git` CLI in this article. Use `git` Bash on Windows, or your standard terminal of choice on Linux or Mac.

Now, create and navigate to a new directory, and run `git init` to initialize a new empty repository in that directory. You should already see a new hidden folder inside your directory called `.git`: this will contain everything `git` needs to keep track of your data.

Which data does `git` keep track of? `git` is designed to track and manipulate one thing alone: diffs, or collections of changes, to files in repositories. These diffs are collected, along with information to describe them, in what `git` refers to as "commits". More on these in the next section. For now, let's start population our *repo*.

### A Bit of Text
A common and exceedingly useful command is `git status`. This command simply asks `git` to tell us what's happening in our directory. This can give a plethora of useful information in all manner of states a repo might be in. The part we care about for now, though, is `git status` will tell us about changes in the repo that have not yet been commited.

To observe this behavior, follow these instructions: first, run `git status` in your empty repo, and observe how it helpfully informs us that there are no commits yet and nothing to commit; next, run `touch file.txt` to create a new empty text file, and run `git status` again. It will now inform us that there exists an untracked file called `file.txt`. `git` tracks files and diffs within files, and "untracked" is simply `git`'s way of telling us this file is not part of this repo yet.

For now, let's start building out our commit. `git` tracks changes in the working directory (that is, the directory you see and manipulate in your file system), and these changes can be either staged, or not. All changes are initially unstaged, and must be added to a so-called "staging area" before they can be commited.

To stage our text file, run `git add file.txt` to add it to the staging area. You can now run `git status` to be informed that, indeed, `file.txt` is staged and ready to be committed. However, before we commit it, let's add some content to our file. Run `echo "some text" >> file.txt` to write "some text" into our file (you can verify this its contents by running `cat file.txt` or opening it in your text editor).

If we run `git status` now, we will see `file.txt` listed both under staged changes *and* unstaged changes! This, of course, is because all `git` does is track diffs, and counts on you to batch them as you see fit; and the only change you staged was adding the empty file, not its new content. Run `git add file.txt` again to add the new changes to our staging area, and verify with `git status`.

### The Coup de Grâce
Now we're ready to actually commit these changes. To commit everything that's currently in the staging area (that is, create a commit containing the accumulated diffs that have been `git add`ed so far), we can use the `git commit` command. The most important part of a commit -- other than the diff contained within -- is what's known as its commit message, a line of text describing its contents.

Running `git commit` now will open your configured text editor to allow you to type in your message, though you may also run `git commit -m "INSERT COMMIT MESSAGE HERE"` to do it all in one go. Having committed our changes, we should now be informed by `git status` that our working tree is clean and there is nothing to commit. Breathe a sigh of deep contentedness: your changes are safe and accounted for.

## Anatomy of Commits
Running `git show` will us details about the latest commit. Doing so now will reveal, at the bottom, the commit's diff (containing our new file and its content), right below the commit's message. Above this we can see the *commit date*, noting the time when we've made the commit a few moments ago. Above that we can see the *commit author*, featuring you (yes, you!). And above these, at the very top, is a line containing our *commit hash*.

*Hashes*, if you're not familiar with the concept, are the product of a computer taking some data and taking it through a *hashing function*, out of which comes a sort of fingerprint, made of a short bit of letters and numbers, representing that original piece of data, and from which the original data cannot be reconstructed. In other words, a hash is a (reasonably) unique identifier to a given clumping of data. And being a *commit hash* -- in our case -- our clumping of data will be the commit, and the resulting hash will serve as a unique identifier of this commit: because any other commit will be comprised of different data, and would therefore have a different hash. But what information is contained in a commit in the first place?

A commit is made of many pieces, and we've already stumbled across some of them! A natural place to start is the *commit message*. A sensible addition that isn't as easy to glean is the commit hash belonging to our commit's **parent** commit, but more on this in the next section. Parts of it are the *author* and *commit date* we saw in the beginning of this section; however, there's also a *author(ing) date*, and the nuance between them is also covered in the next section. The last piece, and perhaps the most important one, is the result of running a *hashing function* on our entire current diff and putting **that** hash in.

These are all the fundamental parts of what makes a commit what it is, and you can't change any of these one bit without changing the *commit hash*. Therefore, many git commands accept *commit hashes*, though they default to the latest one. `git show` works like this, for example; and if you want to run it on a commit different than the latest one, you can easily pass it as in `git show f645eb...`.

## Commit Relationships: An Overview
So far, we've only talked about a single commit. However, a typcical `git` repository can sometimes have hundreds upon hundreds of commits; and especially so when synchronizing multiple local repos between each other (covered next section). We already know many commits can live in relation to each other inside a repo, but how are they related? What do commits mean to each other? And what tools does `git` give us to manage them?

As we've already learned, a fundamental part of any commit is the *`*commit hash*`* of its parent; and as it turns out, this is all that's needed to describe commit relationships: a commit either has a *parent*, or is an *orphan* (e.g. in the case of our initial commit). This means that `git` commits naturally form into sequences, which in `git` are referred to as *branches*. In practice, people use *branches* to track commit sequences and accomplish many things: from a main *branch* they just add to for as long as they care to, through small *branches* that split off from and sometimes `merge` back into the main branch for experiments or bits of segragated work, and even as far as storing separate and auxilliary information such as documentation, build artifacts, or additional dependencies.

In reality, a *branch* in `git` is nothing more than a *reference* -- more commonly spoken about as *refs* -- to some commit. A *branch* also moves along new *commits*: when a new commit is made while we're checked into a *branch* (currently pointing, as it must, on the *commit* that will become the new *commit's parent*), the *branch* is updated to point at our new *commit*. The other kind of *ref* is *tags*, which are similar in function but stay on one *commit* forever.

It's worth noting at this point that `git` only keeps around those commits what are by a *ref*. After some time, all *commits* that aren't *referenced* by anything will be quietly disposed of by `git` to clear up unused space. If you're worried about catastrophic accidents because of this, you shouldn't be because this is very hard to do accidentally. For information or recovering for most catastrophic accidents, see [reflogs](/#reflogs`) below.

The main ways of splitting off and reintegrating, or any other kind of reconcilliation between *branches* are `merge` and `rebase`, which will not be thoroughly covered in this article. The nutshell version of it is that `rebase` reshuffles two sequences of *commits* in some order, by default to "replay" a set of *commits* into the other in chronological order; while `merge` only works on *branches* that were split off, and creates a new *commit* with 2 *parents* containing all the *commits* in the returning *branch*. For both of these, you will probably need to do some amount of *conflict resolution*, i.e. correctly resolving incompatability between files and their contents in the *repo*.

For more on *conflict resolution* and `merge`, see [this](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging#_basic_merge_conflicts) then [this](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging#_basic_merge_conflicts). And see [here](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) for `rebase` and other ways to rewrite history.

For now, we should only cover the humble `git cherry-pick`. Run `git switch --create experiment-branch` to split off a new *branch* from our last commit; then create some new file like so: `echo "successful experiment" >> experiment.txt`, then `add` the file and `commit` it just like before, with a nice message like `"Success! I want this experiment to be merged back in immediatly!` or something to that effect. Now `switch` back to your `main` *branch*, and suppose we really should add that *commit* to our *branch*.

To the rescue comes the humble `git cherry-pick experiment-branch`, which does precisely that! Use `git log --oneline` to verify that indeed, the experimental *commit* was added to this branch. Not only that, but if we use `git log --pretty=fuller` which shows more commit information, we can actually see two slightly different signatures for `Commit` and for `Author`, off by the few moments our *commit* lived only in `experiment-branch`. As is now apparent, `Commit` refers to when this *diff* was *committed*, when it was first put together. And though `Author` starts off the same as `Commit`, it will update as the *commit* is authored around into different places and contexts in the *repo*.

## Repo Relationships

## Useful little tidbits
### reflog
### multiple remotes
### bisect
### zsh aliases

## Conclusion
I hope you now have a deeper, more useful understanding of the way `git` keeps your data safe and flexible and helps you change things fearlessly. With a solid grasp of its fundamentals and a bit of experimentation, you should now be ready to navigate `git`'s terse documentation by yourself. Armed with this knowledge, you are now equipped to use `git` to the advantage of yourself and your team, and to use your tooling for you rather than grappling against it. Happy `git`ting!