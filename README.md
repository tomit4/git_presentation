# Git Presentation

## Introduction

First and foremost, thank you for taking the time to come to my short
presentation on git. This is a small web development group and although we all
know each other, I'll once again shortly introduce myself. My name is Brian
Hayes, I've been a web developer for 3 years and today I'm going to be talking
about the Git Version Control System (VCS).

### Overview

While this presentation is meant to be an overview of Git, in the interest of
covering what I believe is lacking in those of us who are teaching ourselves how
to code, I am going to try my best to quickly cover what I consider to be the basics of
Git, as I am sure the majority of us here have already become at least somewhat
familiar with it in the context of simply saving files to a Github repository.

### What You'll Already Need To Know

Although I will cover briefly these basic concepts, it is expected that you
already:

- Have git already installed on your system, be it Windows, MacOS, or Linux
- Have a familiarity with the essential git commands (git init, git add, git commit, and git push)
- Know how to set up a basic git repository on Github
- What a .gitignore file is and why it's useful

## Features Of Git This Talk Will Not Cover

The subject of git itself is somewhat vast, as there are a myriad of features
available in git, even just solely from the command line and foregoing the
subject of Repository specific features like those available via Github or
Gitlab. Therefore, many advanced features of git, like CI/CD features, will not
be covered in this talk. This talk is mainly concerned with understanding basic
git practices on working with small teams and how the basic flows of
creating/deleting git branches, merging git branches, rebasing git branches, creating
pull requests as well as forking projects on Github. I myself am not
an expert on git, but wanted to give this talk to further my own basic
understanding of git and also to hopefully help all of you should you not be
familiar with these practices. After this presentation, it is my hope that
further discussions will further educate each of us on our own experiences with
git and how to better utilize it to further our own coding best practices.

## What Is Version Control?

In order to first understand git, one must first address the purview under which
git is a subject under, and that is Version Control. According to the [Pro
Git](https://git-scm.com/book/en/v2) book on which this presentation is based
off of, version control is:

> "A system that records changes to a file or set of files over time so that
> you can recall specific versions later."

The most simple method of enforcing version control is to copy your project's
root directory into a separate directory (perhaps with a timestamp) so that should
future changes to your project need to revert back, you can simply utilize this
copied directory. This is what is known as a Local Version Control.

<img src="https://github.com/tomit4/git_presentation/blob/main/assets/slide_001.png">

There are many problems with this system as it takes up an
exorbitant amount of disk space and is a highly error prone approach to version
control.

Thusly, early Version Control Systems(VCS) were created by programmers to
address this common issue of needing to keep track of the history of their
projects. Early Version Control Systems utilized a single server that contained
the versioned files, as well as a record of the number of clients that could
check out files from that central server.

<img src="https://github.com/tomit4/git_presentation/blob/main/assets/slide_002.png">

Eventually, distributed versions of
this model were created to ensure that should one server go down or got
corrupted, a backup of all versions of the project were saved in a network of
databases to ensure fidelity of the project and its version history.

<img src="https://github.com/tomit4/git_presentation/blob/main/assets/slide_003.png">

## What Is Git And Why Is It Everywhere?

Git was created in 2005 by the famous software engineering team over at The
Linux Kernel, including Linux's own creator, Linus Torvaulds.
Prior to 2005, the Linux kernel had used a Version Control System known
as Bitkeeper. The decision to create Git arose out of the breaking down
of the relationship between the Linux community and Bitkeeper, which had
chosen to revoke the tool's free-of-charge status.

The Linux kernel is perhaps the largest open source project in production today,
and thusly a robust version control system was necessary for work to
continue. It turned out that the endeavors Linus and his team would result in a
now ubiquitous version control system known as git.

**Snapshots, Not Differences**

Previous version control systems of the past stored a set
of files and the changes to those files made over time, commonly known as a
delta-based version control.

<img src="https://github.com/tomit4/git_presentation/blob/main/assets/slide_004.png">

Git, on the other hand, utilizes what can be
thought of as a series of snapshots, or a miniature filesystem. By keeping a
record of only the changes to the files made, rather than copying over the
entirity of the project itself, Git is significantly more efficient and
performant than its predecessors. In Git, if a file remains unchanged within an
overarching update to the version of the project, there are not additional changes
saved to the database for that file (only on the files that have been updated).

<img src="https://github.com/tomit4/git_presentation/blob/main/assets/slide_005.png">

**Nearly Every Operation Is Local**

Additionally, because version control is kept locally on each contributor'
s local machine, git saves developer time by not needing to initially reach out
to the project repository unless the developer explicitly pulls in remote changes.

**Git Has integrity**

If one invokes `git log`, one will see a series of SHA-1 40
character hashsum that keep track of all committed changes to the project. This
not only makes referencing a git commit an extremely efficient operation (i.e. O(1)),
it also ensures that corruption to the data within the commit will be nearly impossible
to lose without being detected. This is because the hash is directly tied to the
changes made in that specific commit.

**Git Generally Only Adds Data**

Previous implementations of VCS's allowed for various parts of
the project's version history to be easily deleted and removed. Git generally
only <em>adds</em> data, rather than removing it. While it is possible to
"rewrite" history in git, it is generally more cumbersome to do so and must be
explicitly done. This makes it far easier to recover data that <em>seems</em> lost when
working with Git.

### The Three Stages Of Git

The general workflow of git can be broken down into three states that your files
can reside in:

1. Modified: Files that have yet to be commited to your database.
2. Staged: Files that have been marked as modified, and are ready to go into
   your next commit snapshot.
3. Commited: The data is safely stored in your local database.

<img src="https://github.com/tomit4/git_presentation/blob/main/assets/slide_006.png">

Keep in mind that all of these processes work completely LOCAL to your machine,
there is no interaction with your remote repository in this workflow.

To reiterate, a basic workflow can look like this:

1. You modify files in your working tree.
2. You selectively stage just those changes you want to be part of your next
   commit, which adds <em>only</em> those changes to the staging area.
3. You do a commit, which takes the files as they are in the staging area and
   stores that snapshot permanently in your Git directory.

<img src="https://github.com/tomit4/git_presentation/blob/main/assets/slide_007.png">

### Git Basics

As I mentioned earlier, I will not be covering installing git or configuring git
for the first time on your local machine. There are plenty of [resources
online](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
for how to do that with your specific setup, and the [Pro Git](https://git-scm.com/book/en/v2) book I
mentioned earlier has an in depth chapter on exactly how to do so. Instead this
section will focus more on the basics of a general workflow using Git.

This section will probably be very familiar to you, but it never hurts to have a
refresher on the basics.

To first initialize a git project. You'll need to already be inside your project
directory. Making a basic directory from a bash command line is pretty simple:

```bash
mkdir my_project && cd my_project
```

Once inside of our project, let's just create a basic README.md file and edit it
using our favorite editor (vscode is used here but you can utilize whichever
editor you like):

```bash
touch README.md && code README.md
```

Inside our README file, let's add some simple text:

```markdown
## README.md

This is a basic readme that is being commited to demonstrate the basics of git.
```

Once we have this basic README.md, let's also create a basic .env file:

```text
SECRET="I should not commit this"
```

This is to demonstrate the use of the `.gitignore` file, which is a basic text
file that lists out the files in our project that we do not wish to commit to a
remote repository or version control using git. This usually includes dependency
folders that are rather large, or files that hold onto secret credentials we don't
want exposing to the outside world. Let's create a .gitignore file and quickly add
our .env file to it:

```bash
touch .gitignore && cat "*.env" >> .gitignore
```

Many regular expression expansions such as "\*" will be recognized by .gitignore.
In this case we are simply saying to git, "don't commit any file that ends with
.env"

This tutorial will assume you have already [Created A Github account](https://docs.github.com/en/get-started/quickstart/creating-an-account-on-github) and are somewhat familiar with how to [Create a Repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository).

Once you have created a repository (often titled the same as your project's root directory), you can go about the process of committing these initial files to your repository. First let's simply initialize our project:

```bash
git init
```

This simple command sets up a hidden `.git` directory in the root of your project
directory which holds local configurations for this repository. It now allows us
to utilize the git command line interface specifically within the context of
this project. Let's add our files that we wish to commit.

```bash
git add .
```

Here we are <em>Staging</em> our files for commit. Telling git, "these are the
files I'd like for git to track". Next let's actually commit our changes.

```bash
git commit
```

Now, if we don't provide the usual -m flag we're used to always using, git will
actually open up our editor where we can provide our git message:

```
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch main
#
# Initial commit
#
# Changes to be committed:
# new file: .gitignore
# new file: README.md

:tada: Initial commit!
```

Any line that isn't preceded by a hashtag '#' character will be the commit
message we provide. Normally however, we append the -m flag to the git commit
command so as to provide the commit message inline.

```bash
git commit -m ":tada: Initial commit\!"
```

Now, most of us are probably familiar with this basic work flow. If you're like
me, you're now used to setting up the git remote via the usual instructions
given to us from Github and pushing our changes up to github. It's important to
note, however, that at this point the version control system of git is set up
locally, and we could technically continue to work on our project prior to
setting up our remote and git would indeed keep track of all changes to our
project here. Let's check our git logs to be sure:

```bash
git log
```

Which outputs:

```
commit c3fea393c6691da8c3ada217da5ae70fb07c4b66
Author: [yourname ]<youremail@example.com>
Date:   Thu Feb 1 04:38:42 2024 -0800

    :tada: Initial commit!
```

Let's add a change to our read me so we can further explore this:

```
echo "more text to my readme" >> README.md
```

The git diff command can tell us about what changes we have yet to stage:

```
git diff
```

Which outputs:

```
README.md --- Text
File permissions changed from 100600 to 100644.
1 1 ### README
2 2
3 3 A Basic readme.
  4 more text to my readme
```

With the indented text showing us the line number as well as the text we have
added. Let's stage our README.md file and then commit these changes with a helpful commit message:

```bash
git add -u
```

Prior to committing however, let's also just check on our git status:

```bash
git status
```

Which outputs:

```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   README.md
```

This is showing us that we have modified our README.md file, and in invoking
'git add', we have staged this file for commit. Let's do just that now:

```bash
git commit -m "Added an additional example line to README"
```

If we now invoke git status we'll see the output:

```
On branch main
nothing to commit, working tree clean
```

Meaning that the HEAD of our main branch is up to date, we have no further
changes to our project that git has not yet staged and committed. There is
already much more we can cover in this section, such as unstaging a file,
ammending a commit message, squashing commits, etc. In the interest of keeping
this presentation concise, however, let us now instead move on to briefly cover remotes.

### Git Remotes

Many of us think of git remotes as somehow being the repository we are working
on itself, when in fact remotes are any host where we wish our project to live
<em>elsewhere</em> on a network. While this usually does indeed mean living on a remote
repository hosting provider like Github, it can also simply mean a remote that
lives on our own local machine, or on a Virtual Private Server like Digital
Ocean, etc.

In order to understand remote basics, let's clone a repository we'll be using
later on in this presentation. Find a subdirectory in your home directory that
you are comfortable putting this project and then invoke the git clone command
like so:

```bash
git clone "https://github.com/tomit4/git_sandbox"
```

If we now navigate inside this new project's directory and investigate it's
remotes, we'll see the default remote provided, origin:

```bash
cd git_sandbox && git remote -v
```

The output of this command is:

```
origin	https://github.com/tomit4/git_sandbox (fetch)
origin	https://github.com/tomit4/git_sandbox (push)
```

Here we are being told that whenever we run:

```bash
git fetch origin
```

Or

```bash
git push origin
```

'origin' in this context will expand out to the full url of the remote
repository, https://github.com/tomit4/git_sandbox. There are many ways of
renaming and working with remotes, but this gives us an essential understanding
of the basics, which will have to do for this presentation. Let's end our
understanding of remotes by adding one to our my_project after [Creating a
Repository on Github](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository).

```bash
# inside our my_project directory
git remote add origin "https://github.com/<username>/my_project"
```

And finally let's push our changes up to our new remote:

```bash
git push -u origin main
```

If you now navigate to your repository on Github, you should now see your basic
README.md and .gitignore files within your repository.

This covers the basics of git, and if this was all git had to offer, it would be
a fine version control system in its own right, but what has made git so
ubiquitous in the way it handles branching, which is the topic of the next
section.

### Git Branching

Nearly every VCS has some form of branching support. Branching means you diverge
from the main line of development and continue to do work without messing with
that main line. Usually this is an expensive process, often requiring you to
create a new copy of your source code directory, which can take a loong time for
large projects. Git, on the other hand, takes a more lightweight and unique
approach to branching. It is this feature that has made Git stand out amongst
the rest as the predominant VCS today.

We won't spend too much time on this, but I think it's worth understanding
exactly why Git's approach to branching is so lightweight. Recall the output of
our git log command earlier in the previous section?:

```
commit c3fea393c6691da8c3ada217da5ae70fb07c4b66
Author: [yourname ]<youremail@example.com>
Date:   Thu Feb 1 04:38:42 2024 -0800

    :tada: Initial commit!
```

Our commit sha-1 hash acts as a checksum for each subdirectory, and stores them
as a tree object in the Git repository. Git then creates a commit object that
has the metadata and a pointer to the root project tree so it can re-create that
snapshot when needed.

A branch in Git is simply a lightweight movable pointer to one of these commits.
The default branch name in Git is <em>master</em>. As you start making commits,
you're given a master branch that points to the last commit you made. Each time
you commit, the master branch pointer moves forward automatically.

**Creating A New Branch**

One can create a new branch by simply invoking the git branch command. Let's
create a new branch in our my_project called testing:

```bash
git branch testing
```

We can ensure that testing was indeed created by looking at our branches like
so:

```bash
git branch
```

You'll notice that our HEAD is still pointing to "main" (indicated by the
asterix "\*"), but testing indeed exists.

When we invokeed `git branch testing, we created a new pointer to the same commit
we're currently on. One can check this by using the git log command with a few extra flags for brevity of
output:

```bash
git log --oneline --decorate
```

Which outputs:

```
867a131 (HEAD -> main, testing) Added an additional example line to README
c3fea39 :tada: Initial commit!
```

The HEAD branch is a special branch that specifically refers to the branch we
are currently on, in this case we are still on the main branch, but as you can
see at the end of this parentheses, we have the testing branch, which is showing
as having been created. Let's actually switch over to testing now:

```bash
git checkout testing
```

This moves HEAD to point to the testing branch. If we invoke our git log command
from earlier, we'll see that indeed we've changed over to the testing branch:

```
867a131 (HEAD -> testing, main) Added an additional example line to README
c3fea39 :tada: Initial commit!
```

Let's make a change to our README file, stage those changes, and commit it while on our testing
branch:

```bash
echo "Yet another line we are adding to your README!" >> README.md
```

And now let's stage and commit our changes:

```bash
git add -u && \
git commit -m ":memo: Added another line to readme on testing branch"
```

Let's investigate our git log once again:

```bash
git log --oneline --decorate
```

Which outputs:

```
077d9ed (HEAD -> testing) :memo: Added another line to our readme on testing branch
867a131 (main) Added an additional example line to README
c3fea39 :tada: Initial commit!
```

As you can see, our last commit on testing has now <em>diverged</em> from main.

Because a branch in Git is actually a simple file that contains the 40 character
sha-1 checksum of the commit it points to, branches are cheap to create and
destroy. Creating a new branch is as quick and simple as writing 41 bytes to a
file (4p characters and a newline).

This is in sharp contrast to the way most older VCS tools branch, which involves
copying all of the project's files into a second directory. This can take several
seconds or even minutes, depending on the size of the project, whereas in Git the
process is always intantaneous.

Additionally, because we're recording the parents(the main/master branch) when we commit,
finding a proper merge base for merging is automatically done for us and is
generally very easy to do. These features encourage developers to create and
use branches often.

### Basic Merging

We've already explored the basics of creating a branch. In this case called
"testing". At the point in this tutorial, "testing" is ahead of "main" by one
commit, in which we added yet another additional line to our README. Let's merge
this branch into main. First by switching back over to our main branch using the
checkout command:

```bash
git checkout main
```

Now let's merge our testing branch simply by invoking the git merge command:

```bash
git merge testing
```

We can see that our merge was successful by the output:

```
Updating 867a131..077d9ed
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```

Now that we have merged our testing branch, it is good practice to remove the
old branch like so:

```bash
git branch -d testing
```

We can confirm this once again using the `git branch` command, which outputs:

```
* main
```

### Basic Merge Conflicts

Oftentimes, however, merges don't go so smoothly. As developers work on
different features of the code base, it is common that two or more developers
will change different parts of the same files, but each of them has only pulled
from the same divergent point in the project's history that have now drastically
diverged from each other. Let's simulate this by creating a new branch and
immediately switching to it like so:

```bash
git checkout -b issue_001
```

This is a nice little shorthand for when we want to both create and switch to a
branch in one go. Now, in this issue_001, let's append the following to our
README file like so:

```bash
echo "this line is meant to address issue 001" >> README.md
```

After appending this to our line, let's stage and commit it like so:

```bash
git add -u && \
git commit -m ":memo: Worked on issue 001"
```

Let's now switch back to our main branch:

```bash
git checkout main
```

Keep in mind that our commit on issue 001 has yet to be merged into main, we
have taken our HEAD and "travelled back in time" in our version history. Now,
let's play the role of another developer here and work on issue 02. Let's
create and switch to a new branch again:

```bash
git checkout -b issue_002
```

And here let's do a similar work on our README file like so:

```bash
echo "this line is meant to address issue 002" >> README.md
```

And again, let's stage and commit these changes:

```bash
git add -u && \
git commit -m ":memo: Worked on issue 002"
```

Now think on this for just a moment, both branches have worked on the same file.
Ideally, the project lead will pull in each change one by one and they should
all line up, right? Let's see what happens when we return to main and merge our
changes from issue 001:

```bash
git checkout main
```

And now merge the changes in issue_001:

```bash
git merge issue_001
```

The output shows us it was successful:

```
Updating 077d9ed..079e926
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
```

Let's now merge in issue_002:

```bash
git merge issue_002
```

Which presents us with the following output:

```
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

Let's investigate this further using `git status`, which outputs:

```
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Anything that has merge conflicst and hasn't been resolved is listed as
unmerged. Git adds standard conflict-resolutioon markers to the files that have
conflicts, so you can open them manually and resolve those conflicts. Let's
investigate our README file to see it for ourselves. Opening our README in our
editor reveals this:

```
### README

A Basic readme.
more text to my readme
Yet another line we are adding to your README
Yet another line we are adding to your README
<<<<<<< HEAD
this line is meant to address issue 001
=======
this line is meant to address issue 002
>>>>>>> issue_002
```

As you can see, the HEAD (the branch we currently are on), shows this line as
being "this line is meant to address issue 001", but when we merged in branch
issue_002, this same line was changed to "this line is meant to address issue
002". Rather than overwrite these changes, git pauses, and asks us to edit these
demarcations to ensure we pick the right piece of code. Let's say we're the
project lead and we see this code and we determine that the code addressing
issue 001 is safe to remove as the code being brought in from issue 002 doesn't
break anything and adds a needed feature. Thusly we would edit out our file so
that none of the demarcations left by our merge conflict remain and only the
changes we want to keep remain. This would leave our README file looking like
so:

```
### README

A Basic readme.
more text to my readme
Yet another line we are adding to your README
Yet another line we are adding to your README
this line is meant to address issue 002
```

We can now stage and commit our files, which will complete our merge:

```bash
git add -u &&\
git commit -m ":memo: Merged in issue 002"
```

Note that we created a new commit for this merge, which will add onto our
version history. This is an important distinction to make when compared to the
slightly more complicated process of rebasing. Let's investigate our git logs to
see our version history thus far:

```bash
git log --online --decorate
```

Which outputs:

```
c485e07 (HEAD -> main) :memo: Merged in issue 002
026aeed (issue_002) :memo: Worked on issue 002
079e926 (issue_001) :memo: Worked on issue 001
077d9ed :memo: Added another line to our readme on testing branch
867a131 Added an additional example line to README
c3fea39 :tada: Initial commit!
```

While this is very clear and concise, the purpose of version control is to tell
an easily followed <em>story</em> of how the project was created. It is
important that commit messages are both brief but also descriptive. Too many
commit messages can clutter the history of the project and not provide a good
summary of where the project was at in that current place in history. This is
why squashing and ammending commit messages is an important part of working with
git (that unfortunately we will not have time to cover in this presentation but
is worth looking into of your own accord when you have time).

But let's say that we didn't want to have a separate commit "Merged in issue 002",
rather we wanted simply to apply the last commit "Worked on issue 002" on
<em>top</em> of our last commit to "main"? This is where we would utilize one of
git's other major features, rebasing.

### Basic Rebasing

Rebasing is an alternative to merging which allows us to "rewrite" our version
history by overwriting the current place of our history (the HEAD) with the
latest commit from a divergent branch. Let's demonstrate by creating a similar
scenario as before. Let's create a branch issue_004, and make some changes as
before:

```bash
git checkout -b issue_003
```

And now let's append another line to the README:

```bash
echo "this change demonstrates rebase for issue_003" >> README.md
```

Let's stage and commit these changes to our new issue_003 branch:

```bash
git add -u && \
git commit -m ":memo: Made changes to README for issue_003 rebase"
```

And let's go back to our main branch:

```bash
git checkout main
```

And now let's create another branch issue_004, switch over to it...:

```bash
git checkout -b issue_004
```

And change the same README file in a similar fashion:

```bash
echo "this change demonstrates rebase for issue_004" >> README.md
```

And again, stage those changes and commit them:

```bash
git add -u && \
git commit -m ":memo: Made changes to README for issue_004 rebase"
```

Instead of returning to our main branch, we'll instead remain in our issue_004
branch and rebase it <em>on top of</em> our main branch:

```bash
git rebase main
```

Which outputs:

```
Current branch issue_004 is up to date.
```

If we now switch to our issue_003 and attempt the same:

```bash
git checkout issue_003
```

And rebase ...

```bash
git rebase main
```

We'll get a similar output:

```
Current branch issue_003 is up to date.
```

If we then return to main and attempt to merge issue_003, we'll perform a
fast-forward merge:

```bash
git checkout main
```

And merge...

```bash
git merge issue_003
```

We'll see a successful merge output:

```
Updating c485e07..b462b90
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
```

Let's now do the same with issue_004:

```bash
git merge issue_004
```

Which outputs our conflict as we expected:

```
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

Let's fix it, if we open our README, we'll see a similar pattern as before:

```
### README

A Basic readme.
more text to my readme
Yet another line we are adding to your README
Yet another line we are adding to your README
this line is meant to address issue 002
<<<<<<< HEAD
this change demonstrates rebase for issue_003
=======
this change demonstrates rebase for issue_004
>>>>>>> issue_004
```

Let's once again just take our latest change from issue_004, assumming it
doesn't break anything and will work with the rest of our code. This will leave
our README as such:

```
### README

A Basic readme.
more text to my readme
Yet another line we are adding to your README
Yet another line we are adding to your README
this line is meant to address issue 002
this change demonstrates rebase for issue_004
```

Let's add it as before and commit it. this time with no message applied:

```bash
git add -u && \
git commit
```

If we then investigate our logs, we'll see this output:

```bash
git log --online --decorate
```

You'll see this output:

```
f6b4088 (HEAD -> main) Merge branch 'issue_004'
40ced1f (issue_004) :memo: Made changes to README for issue_004 rebase
b462b90 (issue_003) :memo: Made changes to README for issue_003 rebase
c485e07 :memo: Merged in issue 002
026aeed (issue_002) :memo: Worked on issue 002
079e926 (issue_001) :memo: Worked on issue 001
077d9ed :memo: Added another line to our readme on testing branch
867a131 Added an additional example line to README
c3fea39 :tada: Initial commit!
```

This may look trivially the same, but think carefully on the workflow of rebase.
The major difference is that we rebased from within the branch "we wished to
apply" over the main branch, not merging our issue branch FROM the main branch.

Rebasing essentially tells git that whatever changes we made in the
CURRENT branch we're on, to APPLY them <em>on top</em> of the "main" branch. This
as opposed to merging, in which we take the endpoint (last commit) of the branch
and merges it into the main branch instead.

To be honest, the distinction between merging and rebasing can seem confusing to
me as I am a beginer, and accounts I've read online indicate that it takes quite
a bit of encountering both on a project before one can appreciate the
differences between the two. So please take the above explanation with a grain
of salt.

### Rebase vs Merging

The majority of this document has been me simplifying aspects of the [Pro Git](https://git-scm.com/book/en/v2) book using examples that I wrote myself. When it came to writing the Rebase vs Merge, however, I thought it might be more prudent to simply quote the book as it gives an excellent overview of which is better. Spoiler alert, it depends:

> Now that you've been rebasing and merging in action, you may be wondering
> which one is better. Before we can answer this, let's step back a bit and talk
> about what history means.
>
> One point of view on this is that your repository's commit history is a <b>record of
> what actually happend.</b> It's a historical document, valuable in its own
> right, and shouldn't be tampered with.
>
> From this angle, changing the commit history is almost blasphemous; you're
> <em>lying</em> about what actually transpired. So what if there was a messy
> series of merge commits? That's how it happend, and the repository should
> preserve that for posterity.
>
> The opposing point of view is that the commit history is the <b>story of how
> your project was made</b>. You wouldn't publish the first draft of a book, so
> why show your messy work? When you're working on a project, you may need a
> record of all your missteps and dead-end paths, but when it's time to show
> your work to the world, you may want to tell a more coherent story of how to
> get from A to B. People in this camp use tools like `rebase` and
> `filter-branch` to rewrite their commits before they're merged into the
> mainline branch. They use tools like `rebase` and `filter-branch`, to tell the
> story in the way that's best for future readers.
>
> Now, to the question of whether merging or rebasing is better: hopefully
> you'll see that it's not that simple. Git is a powerful tool, and allows you
> to do many things to and with your history, but every team and every project
> is different. Now that you know how both of these things work, it's up to you
> to decide which one is best for your particular situation.
>
> You can get the best of both worlds: rebase local changes before pushing to
> clean up your work, but never rebase anything that you've pushed somewhere.

## Conclusion

This short presentation only begins to cover the basics of working with git from
the command line. You'll notice we didn't work with Github almost entirely
within this tutorial, and that's to hammer home the point that git works as a
version control system on <em>your</em> local machine. Pushing your changes to
a remote repository like Github, Gitlab, or others is an ecosystem on its own.

You may have noticed that during the course of this presentation, I have asked
you to clone a repository of mine called `git_sandbox`. The next part of this
presentation will be more free form as we'll cover the basics of pull requests
and merging via Github. Obviously this presentation has already been quite
lengthy, and if you'd rather have further discussions on what was covered here
now rather than do the interactive exercises, I'd of course yield the floor back
to all of you to decide how you'd like to proceed.
