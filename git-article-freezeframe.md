# Diff Graphs in Git
Git is a wonderful beautiful piece of software, used by millions of professionals around the world; but far too many of us only know how to operate it through arcane rituals and rote memorization, lacking instruction and insight on its inner workings.

This article aims to give you a working framework of knowledge concerning Git. Comparisons between Git and other VCS tools is out of scope for this article: those are straightforward enough to find, and if you're here you're probably already sold on the merits of Git through osmosis if nothing else. For the sake of inclusivity, this article doesn't assume any other previous knowledge or experience using Git.

If you're still curious to establish or expand your understanding of the way this beautiful tool works for you, read on, read on.

## Baby's First Commit
Everything Git does and cares about lives inside a repository. We'll elaborate on what repositories -- or colloquially repos -- are, and what they contain, in a moment. For now, let's create a fresh local repo on your machine. Start by [installing Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and performing the [first-time setup](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), if you haven't already.

There are a few ways to act on Git repos: some people prefer using a desktop app with a GUI that lets them run predefined actions in repos; but for greater control and deeper understanding, we'll use our terminal and the Git CLI in this article. Use Git Bash on Windows, or your standard terminal of choice on Linux or Mac.

Now, create and navigate to a new directory, and run `git init` to initialize a new empty repository in that directory. You should already see a new hidden folder inside your directory called `.git`: this will contain everything Git needs to keep track of your data.

Which data does Git keep track of? Git is designed to track and manipulate one thing alone: diffs, or collections of changes, to files in repositories. These diffs are collected, along with information to describe them, in what Git refers to as "commits". More on these in the next section. For now, let's create our first commit.

A common and exceedingly useful command is `git status`. This command simply asks Git to tell us what's happening in our directory. This can give a plethora of useful information in all manner of states a repo might be in. The part we care about for now, though, is `git status` will tell us about changes in the repo that have not yet been commited.

To observe this behavior, follow these instructions: first, run `git status` in your empty repo, and observe how it helpfully informs us that there are no commits yet and nothing to commit; next, run `touch file.txt` to create a new empty text file, and run `git status` again. It will now inform us that there exists an untracked file called `file.txt`. Git tracks files and diffs within files, and "untracked" is simply Git's way of telling us this file is not part of this repo yet.

For now, let's start building out our commit. Git tracks changes in the working directory (that is, the directory you see and manipulate in your file system), and these changes can be either staged, or not. All changes are initially unstaged, and must be added to a so-called "staging area" before they can be commited.

To stage our text file, run `git add file.txt` to add it to the staging area. You can now run `git status` to be informed that, indeed, `file.txt` is staged and ready to be committed. However, before we commit it, let's add some content to our file. Run `echo "some text" >> file.txt` to write "some text" into our file (you can verify this its contents by running `cat file.txt` or opening it in your text editor).

If we run `git status` now, we will see `file.txt` listed both under staged changes *and* unstaged changes! This, of course, is because all Git does is track diffs, and counts on you to batch them as you see fit; and the only change you staged was adding the empty file, not its new content. Run `git add file.txt` again to add the new changes to our staging area, and verify with `git status`.

Now we're ready to actually commit these changes. To commit everything that's currently in the staging area (that is, create a commit containing the accumulated diffs that have been `git add`ed so far), we can use the `git commit` command. The most important part of a commit -- other than the diff contained within -- is what's known as its commit message, a line of text describing its contents.

Running `git commit` now will open your configured text editor to allow you to type in your message, though you may also run `git commit -m "INSERT COMMIT MESSAGE HERE"` to do it all in one go. Having committed our changes, we should now be informed by `git status` that our working tree is clean and there is nothing to commit. Breathe a sigh of deep contentedness: your changes are safe and accounted for.

## Anatomy of Commits

## Commit Relationships

## Useful little tidbits
### reflog
### multiple remotes
### bisect
### zsh aliases

## Conclusion
I hope you now have a deeper, more useful understanding of the way Git keeps your data safe and flexible and helps you change things fearlessly. With a solid grasp of its fundamentals and a bit of experimentation, you should now be ready to navigate Git's terse documentation by yourself. Armed with this knowledge, you are now equipped to use Git to the advantage of yourself and your team, and to use your tooling for you rather than grappling against it. Happy Gitting!