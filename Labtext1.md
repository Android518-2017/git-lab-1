git is a client server version control system created in 2005 by Linus
Torvalds.

The client is the git command the server is any repo server
<http://github.com>, <https://bitbucket.org/> <http://gitlab.com> etc.

The examples in here use github but could easily be swapped for
bitbucket or gitlab.

If you already know how to use git but can't remember some sequence
check here [git quick ref](git_quick_ref "wikilink") \_\_FORCETOC\_\_
This is all command line, if you re having trouble remembering the bash
command line here are some
[references](https://drive.google.com/open?id=0B-CHlg81QPjfVU5PSkxYM1hsSEE)
<http://bit.ly/bashqr>

Set up a client side repo (local)
---------------------------------

Configure your git client side (the repo will exist first locally then
also remotely)\
open a git-bash command line window & configure your client (In the labs
this will configure the git client info in the H: drive) If you are
running on Linux just make sure git is installed.

If this is your first time using git, then set up the config:

``` {.bash}
$ git config --global user.name "Grace Hopper"
$ git config --global user.email "gracie@ilovelinux.ca"
$ git config --global color.ui "auto"
```

To check the settings (this or next time)

``` {.bash}
$ git config --list
```

To get help

``` {.bash}
$ git config -h 
$ git config --help 
```

We are going to make a fake repository (on the client side) Do this on
the h: drive (from <http://swcarpentry.github.io/git-novice/03-create/>)

``` {.bash}
$ mkdir planets
$ cd planets
```

make the directory into a git repository , cwd is h:\\planets

``` {.bash}
$  git init .
Initialized empty Git repository in planets/.git/

$ ls -a   
.  ..  .git
$ git status
```

``` {.bash}
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git add" to track)
```

Track changes, create a file in the planets directory from
<http://swcarpentry.github.io/git-novice/04-changes/>

``` {.bash}
$ vi pluto.txt   #  put some text in here "alas it is no more"  & save it
$ git status 
```

``` {.bash}
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#   pluto.txt
nothing added to commit but untracked files present (use "git add" to track)
```

tell git to track our file

``` {.bash}
$ git add pluto.txt
$ git status
```

``` {.bash}
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#   new file:   pluto.txt
#
```

tell git to take a permanent copy of our new file

``` {.bash}
$ git commit -m "first planet file, never forget the one that got away Pluto"
```

``` {.bash}
[master (root-commit) f22b25e] first planet file, never forget the one that got away Pluto
 1 file changed, 1 insertion(+)
 create mode 100644 pluto.txt
```

make sure it committed ok

``` {.bash}
$ git status
```

``` {.bash}
# On branch master
nothing to commit, working directory clean
```

all commits in reverse chronological order:

``` {.bash}
$ git log
```

``` {.bash}
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Grace Hopper <gracie@ilovelinux.ca>
Date:   Thu Aug 22 09:51:46 2013 -0400

    first planet file, never forget the one that got away Pluto
```

modify the file

``` {.bash}
$ vi pluto.txt   #  add a new line of text "we miss you pluto"  & save it
$ git status 
```

``` {.bash}
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#   modified:   pluto.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
```

review the changes before saving them, git knows that it doesn't match
what is in the local repo

``` {.bash}
git diff
```

Output of git diff:

``` {.bash}
diff --git a/pluto.txt b/pluto.txt
index 9f83956..0ce4e22 100644
--- a/pluto.txt
+++ b/pluto.txt
@@ -1 +1,2 @@
 alas it is no more 
+we miss you pluto
```

so now try to commit

``` {.bash}
$ git commit -m "Add sentiment"
On branch master
Changes not staged for commit:
    modified:   pluto.txt

no changes added to commit

$ git status
```

NOPE! we need to add it before committing

``` {.bash}
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#   modified:   pluto.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
```

add then commit

``` {.bash}
$ git add pluto.txt
$ git commit -m "Add sentiment"
```

Success!

``` {.bash}
[master 340b5d6] add sentiment
 1 file changed, 1 insertion(+)
```

So getting files into a repo is a 2 part process: untracked changes
--(git add)-&gt; staging area --(git commit)--&gt; repository

![](git-staging-area.jpg "git-staging-area.jpg")

Set up a server side repo (remote)
----------------------------------

For this exercise we are going to use github which has only public repos
for free, for private repos create a bitbucket account.

1.  If you don't have a github account create one
    <https://github.com/join?source=header-home>
2.  Once you have created your account logon, to create a new repo click
    on the plus sign:\
    ![](Create-repo-github.png "fig:Create-repo-github.png")\
    \
3.  Select new repository then fill in the info & click on the green
    button (Do NOT create a README):\
    ![](Create-repo-github2.png "fig:Create-repo-github2.png")
4.  If the repo is created properly you will see this;\
    ![](Create-repo-github3.png "fig:Create-repo-github3.png")
    -   After the first part you had a local repo called planets, now
        you have an empty repo called planets on the github server\
        effectively you just did this on github:
        ``` {.bash}
        mkdir planets;cd planets;git init . 
        ```

Connect your client side (local) repo to your server side (remote) repo
-----------------------------------------------------------------------

What we need to do to connect them is to tell the client side (local)
that a github.com repo is the server side (remote). It is better to use
ssh but you will need to set up keys for that, https you can use
userid/password. So we'll use https for now.

Note server side == remote && client side == local

Copy the https url into your clipboard\
![](Create-repo-github4.png "fig:Create-repo-github4.png")

Go to your local repo directory (git-bash command line)

``` {.bash}
$ cd planets
$ git remote add origin https://github.com/campbe13/planets.git
$ git remote -v # verify it
origin  https://github.com/campbe13/planets.git (fetch)
origin  https://github.com/campbe13/planets.git (push)
```

Now the local (client) repo knows about the remote (server) repo, let's
sync them.

``` {.bash}
$ git push origin master
Username for 'https://github.com': campbe13
Password for 'https://campbe13@github.com': 
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (6/6), 510 bytes | 0 bytes/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To https://github.com/campbe13/planets.git
 * [new branch]      master -> master
```

Now the contents of the local repo are on the remote, reload the page:\
![](Create-repo-github5.png "fig:Create-repo-github5.png")

Add a README.md, for now just put in some basic text, you can get more
sophisticated later, with markdown
<https://help.github.com/articles/basic-writing-and-formatting-syntax/>

You can use this method to add any files to your own repos.

``` {.bash}
tricia@ubbie:~/planets$ echo "My first repo" > README.md
tricia@ubbie:~/planets$ git add README.md 
tricia@ubbie:~/planets$ git commit -m "add readme"
[master ef2c0b8] add readme
 1 file changed, 1 insertion(+)

 create mode 100644 README.md
tricia@ubbie:~/planets$ git push origin master
Username for 'https://github.com': campbe13
Password for 'https://campbe13@github.com': 
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 284 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/campbe13/planets.git
   340b5d6..ef2c0b8  master -> master
```

Reload the page it will show you the README.md

Using your repo from a new computer
-----------------------------------

*You can do this part from home or from your laptop.*

Again this example uses github but any remote server repo will work
similarily.

This is for anywhere you are using a new computer or want to use a clean
check out of you repository from the remote (github).

So you created a repo and the contents are the same on the lab computer
h: drive repo and on the remote github repo. You go home and you want to
use the same repo on another computer.

Hopefully you are doing this on Linux, otherwise you will have to first
install git-bash @ home <https://git-scm.com/downloads>

*<font color="#0000ee">*'This is where you may want to set up ssh keys,
I will show you that separately. *'</font>*

When I clone it I get a copy of the whole directory & config from the
remote server.

``` {.bash}
$ git clone https://github.com/campbe13/planets.git 
Cloning into 'planets'...
remote: Counting objects: 9, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 9 (delta 0), reused 9 (delta 0), pack-reused 0
Unpacking objects: 100% (9/9), done.
Checking connectivity... done.

$ cd planets
$ ls -l
total 8
-rw-rw-r-- 1 tricia tricia 38 Aug 28 19:51 pluto.txt
-rw-rw-r-- 1 tricia tricia 14 Aug 28 19:51 README.md
```

modify some files, and/or create new ones, work with the directory I
added home-planet.txt and changed README.md

``` {.bash}
$ vi home-planet.txt
$ ls -l
total 12
-rw-rw-r-- 1 tricia tricia 32 Aug 28 19:54 home-planet.txt
-rw-rw-r-- 1 tricia tricia 38 Aug 28 19:51 pluto.txt
-rw-rw-r-- 1 tricia tricia 14 Aug 28 19:51 README.md
$ vi README.md 
$ git diff
diff --git a/README.md b/README.md
index b85b88f..cc8bdcd 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,3 @@
 My first repo
+
+modified Saturday to fix the thingy error
$ git add .
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:   README.md
    new file:   home-planet.txt
```

remember add will update the staging area, commit will update the local
repo

``` {.bash}
$ git commit -m "done for today will work from school tomorrow"
[master c3993b1] done for today will work from school tomorrow
 2 files changed, 3 insertions(+)
 create mode 100644 home-planet.txt
```

If I want to use it from school tomorrow I need to update the remote
repo

``` {.bash}
$ git push origin master
Username for 'https://github.com': campbe13
Password for 'https://campbe13@github.com': 
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 432 bytes | 0 bytes/s, done.
Total 4 (delta 0), reused 0 (delta 0)
To https://github.com/campbe13/planets.git
   ef2c0b8..c3993b1  master -> master
```

Now the remote (server) github repo contains my latest changes.

Using your repo again
---------------------

So you've used the repo at school (labs, H: drive) and at home and you
come back to one of those computers and want to update the current repo.
(For example you made a change at home, want to use it at school. You
made a change at school want to use it at home.)

In other words you have a local copy of the repo but it's not up to
date. You have been keeping the remote (github) up to date (best
practice.)

Before you do anything with your code, make sure you update your local
repo with any changes from the remote (github)

``` {.bash}
$ ls 
pluto.txt  README.md
$ git pull origin master 
From https://github.com/campbe13/planets
 * branch            master     -> FETCH_HEAD
Updating ef2c0b8..c3993b1
Fast-forward
 README.md       | 2 ++
 home-planet.txt | 1 +
 2 files changed, 3 insertions(+)
 create mode 100644 home-planet.txt
```

Now your local repo matches the remote (gitub) you can work on this,
don't forget to update the remote again as needed & when you are
finished.

``` {.bash}
$ ls -l
total 12
-rw-rw-r-- 1 tricia tricia 32 Aug 28 20:07 home-planet.txt
-rw-rw-r-- 1 tricia tricia 38 Aug 28 17:12 pluto.txt
-rw-rw-r-- 1 tricia tricia 57 Aug 28 20:07 README.md
```

I added important-work.txt and updated home-planet.txt

``` {.bash}


$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   home-planet.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    important-work.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

So I need to add the new file (stage), then commit them (local repo)

``` {.bash}
$ git add .
$ git commit -m "important work done"
[master f843520] important work done
 2 files changed, 3 insertions(+)
 create mode 100644 important-work.txt
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
nothing to commit, working directory clean
```

Then I need to keep the remote (github) up to date

``` {.bash}
$ git push origin master
Username for 'https://github.com': campbe13
Password for 'https://campbe13@github.com': 
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 440 bytes | 0 bytes/s, done.
Total 4 (delta 0), reused 0 (delta 0)
To https://github.com/campbe13/planets.git
   c3993b1..f843520  master -> master
```

Collaborate using a git repo
----------------------------

Again this example uses github but any remote server repo will work
similarly. This is collaboration where both contributors have access.

This is the shared repository model: collaborators have push access to a
shared repo.

Do this with a partner, each of you act as owner and collaborator, do it
twice, so that you each contribute to your partner's repo, also create a
file that is <yourname>.txt:
<http://swcarpentry.github.io/git-novice/08-collab/>

Note when you do this if you are each using a repo with the same name
you must do it in a separate directory, do not cross the streams!!!!!

For example:

Sam, logged on at Dawson:

``` {.bash}
 h:\git\planets 
```

Tom, logged on at Dawson:

``` {.bash}
 h:\git\planets 
```

Sam sharing Tom's

``` {.bash}
cd   h:\git\shared\  
git clone <Tom's url> # creates h:\git\shared\planets
```

Tom sharing Sam's

``` {.bash}
cd h:\sharedstuff\  
git clone <Sam's url> # creates h:\sharedstuff\planets
```
