Notes from Git Tutorial
=======================

These notes were taken while watching the awesome introduction to git at [link](https://www.youtube.com/playlist?list=PLOJrc9IhPiwUxmNXE4371hmdouVKIeoLV).
Note: command line instructions are in **bold**


Git principles
--------------
CVS was an improvement because it was the first to allow concurrent editing
SVN (from Apache) was an improvement because it was directory-based so you dould move files

Git was an imrpovement because it was a distributed version control system---users each have their own repo so data is redundant. Other systems are central version control systems so users check out and commit code to the centralized one. In Git, the central repo is just a convention---you could theoretically have three different 'main' repo's that each have their own purposes.

Git also differs because instead of tracking version of files, it tracks changes to files as 'change sets' or 'patches' that are exchanged between distributed repositories. This makes collaboration especially easy because repo's can be forked and changes can be accepted as desired

Other VCS's use a 2 Tree Architecture:
[working repo] ->commit-> [repository] ->update-> [repository] etc

Git uses a 3 Tree Architecture:
[working repo] ->add-> [staging index] ->commit-> [repository]
This allows you to create change sets with a bunch of similar files with one commit

Git generates a checksum (produced with SHA-1 hash algorithm---a 40 character hexidecimal string 0-9, a-f)
Changing the data will always produce a differed SHA/checksum

Git's HEAD value points to a specific commit in the currently checked-out repo (the tip of the repo). This is where the next changes/commit will take place

Git basics
----------
Adding auto complete to .bash_profile  
Using the help pages (f=forward, b=back, q=quit to navigate)  
**git help**  

### Deleting, moving and renaming files
Best to use the git equivalents so you don't have to use git add:  
**git rm filename**  
**git mv filename newfilename**  

### Add and commit a file at the same time  
**git commit -am 'message'**  

### Seeing changes
**git log**  
is the same thing as  
**git log HEAD**  
**git log --oneline**  

**git diff**  
will return all the changes in the working directory. Use  
**git diff --staged**  
to return changes in the staging area  

If you need the lines to wrap, use option for the less command:  
**plus-key capital-S enter-key**  

See the changes on the same line, differentiated with colors:  
**git diff --color-words filename**  



Undo
----




Branches
--------


Stashing
--------



Merging and merge conflicts
---------------------------



Remotes
-------

### Fetch changes to branch from a remote repository
Fetch synchronizes origin/master with whatever's on the remote repository: don't forget that this is not automatic and it will not effect your local master branch, it just syncs up origin/master (which is supposed to be a copy of the remote repo)  
**git fetch origin**  
if you only have one remote repository, can abbreviate to:  
**git fetch**  

### Basic fetch guidelines:
* fetch before you work
* fetch before you push
* fetch often! especially before working offline

Since fetch doesn't make changes to your working master branch, you'll have to use merge for that

### Merging in fetched changes
Origin/master is like any other branch, only it's a branch that git is in charge up--since it needs to stay in line with what's on the remote repo, you're not allowed to make commits to it (you commit to your own branches, master, etc.)
To merge the changes from origin/master in with your master branch:  
first remind yourself which branches there are:  
**git branch -a**  
Then take a look at what's different between master and origin/master:  
**git diff origin/master..master**  
Looks good?  
**git merge origin/master**  

### But there's a shortcut!
Git pull = git fetch + git merge  
BUT only use it if you understand what that fetch and pull will be doing to your master branch.  

### How to check out remote branches
So you type  
**git branch -r**  
and you see that there's a remote branch you want to work with  
Since you can't check out remote branches directly (git doesn't want you messing with them)  
BUT you can create a branch from them that you then work with  
**git branch my-interesting-branch HEAD**  
earlier you'd have chosen HEAD because that points to the default, master branch  
but now it's some other branch so you do this:  
**git branch my-interesting-branch origin/interesting-branch**  
it will automatically be set up to track the remote one  
You can also do it this way though (which will create and checkout the branch):  
**git checkout -b my-interesting-branch origin/interesting-branch**  

### Pushing to a remote server that's been updated
whenever there are new commits (even if they don't specifically conflict) git will want you to fetch, merge origin/master and merge those changes, then push  
SO if git doesn't like you pushing, do a fetch, then merge, then try pushing again  

### How to delete a remote branch
first way:  
**git push origin :branch-to-delete**  
why a weird colon? because technically   
**git push origin branch-to-push**  
is actually short for:  
**git push origin branch-to-push(ie, local branch):branch-to-push(ie, branch-that-will-be-on-remote)**  
second (new) way:  
**git push origin --delete branch-to-delete**  

### How to collaborate
on github, you'd navigate to the repo and click on settings/collaboration to add usernames  
this lets people clone and read, write to the repo  
but what about open source projects where people shouldn't automatically have write access? -> create a fork  
if you want to create a fork, first decide what you'll be contributing  
check the network and issues page for the repo to make sure it's not already being worked on  
if not, create an issue to stake your claim to solving the issue  
then fork the project, work on it in your local area  
when you're ready to commit the changes back to the origin/master, you create a pull request with a message about your new code  
**git checkout master**  
**git fetch**  
**git merge origin/master**  
do work on your new feature:  
**git checkout -b new_feature**  
**git add changed-files**  
**git commit -m "this commit creates my new feature"**  
**git fetch**  
**git push -u origin new_feature**  
this is when you email your collaborator to let them know about the new feature branch to look at  

your collaborator then makes sure she's on the master branch:  
**git checkout master**  
**git fetch**  
**git merge origin/master**  
good practice to do a fetch/merge at this point  
**git checkout -b new_feature origin/new_feature**  
**git log**  
**git show sha-id**  
maybe collborator wants to add something to it, so she makes a change  
**git commit -am "added whatever"**  
**git fetch**  
**git push**  
collaborator emails you back about the addition, you go ahead and take a look:  

**git fetch**  
**git log -p new_feature..origin/new_feature**  
like the changes?  
**git merge origin/new_feature**  
now everyone has the same stuff for new_feature branch  
ready to fold it back into the master branch:  
**git checkout master**  
**git fetch**  
**git merge origin/master**  
**git merge new_feature**  
**git push**  
now the new changes have been added to the main master remote repo, woohoo!  

Next Steps
----------

### Aliases
you'll probably want to have them in your global git config  
~/.gitconfig  
**git config --global alias.st "status"**  
to look at it:  
**cat ~/.gitconfig**  
can also string them together like:  
**git config --global alias.al1 "alias 1" alias.al2 "alias 2"**  

Some very common ones:
**git config --global alias.co checkout**  
**git config --global alias.ci commit**  
**git config --global alias.br branch**  
**git config --global alias.df diff**  
**git config --global alias.dfs "diff --staged"**  
need the double quotes above b/c there's a space  
**git config --global alias.logg "log --graph --decorate --oneline --abbrev-commit --all"**  

### Password caching
either use a keychain program (see github documentation)  
or generate ssh keys (again see github documentation)  

