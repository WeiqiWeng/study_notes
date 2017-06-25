# Git Workflow

This note works as a quick introduction of how to do simple version control with Git. There are many code repository system. We use *GitHub* as an example.

## Create New Project on *GitHub*

First, we create new project through *GitHub* UI and call the project git_workflow. Then we need to enter the directory containing our project.
```sh
cd git_workflow
```
Secondly, initialize git, add remote repository we just created on *GitHub* and call the remote repository origin.
```sh
git init
git add -all
git commit -m "initialize git version control"
git remote add origin https://github.com/WeiqiWeng/git_workflow.git
git push -u origin master
```
Each shell does the corresponding work:
* initialize Git, this step will automatically create branch *master*
* add **all** files in the current local repository to version control
* commit the change
* add remote repository from the **url** and name it as origin
* push to origin and set origin as the upperstream of branch master.

Sometimes you can use 
```sh
git remote rename A B
```
to rename your remote branch A to B.


## Update the Project

If we want to update the project, such as adding new feature or fixing a bug, the better practice is to create a new local branch and merge to *master* at last. In this way, the team can make sure the version in branch *master* is always ready for production and not affected when doing update.

The following commands create a new branch and switch to it.
```sh
git branch update_the_project
git checkout update_the_project
```

Then we may do updates in this branch and upload to remote repository. Before this, remember to set upper stream to the current branch. Here we still set *origin* as the up stream of branch *update_the_project*.
```sh
## in branch update_the_project
git add .
git commit -m "write new section for update the project"
git push --set-upstream origin update_the_project
```

If you do further change after just committed and you don't want to make two separate commites, you can use
```sh
git commit ammend
```
to append new content to the last commit. Also the command 
```sh
git show *commit_id*
```
shows the commit message of *commit_id*. 

Make sure the new feature passes all the tests and works as expected. Then we can fetch the latest version of remote branch, merge to branch *master* and finally put to production.
```sh
git chechout master
git fetch
git merge update_the_project
# git merge update_the_project --squash
git branch --delete update_the_project
```
Each shell does the corresponding work:
* switch to branch *master*
* merge *master* and *update_the_project*
* sometimes you may want to use --squash option to combine your commits into one and then merge into main branch
* delete the branch for sanity.

You can also use 
```sh
git pull 
```
instead of 
```sh
git fetch 
```
followed by
```sh
git merge
```
.
It's a shorthand for the latter two commands.

If we have serveral changes committed in the sub-branch and we want to merge these commits with the commits in branch *master* at once, we can use
```sh
git chechout master
git merge update_the_project --squash
```
.

We can delete the branch as illustrated above.

## Deal with Multiple Commits

In this case, you've committed several times before you actually do any push or merge. however you find something wrong with the current version and wish to go back. The first thing you need to do is to heck your commit history.
```sh
git log --oneline --graph
```
will list all the commits from the most recent one to the oldest one. The option --oneline shows each commit with a line of abstraction while --graph shows each branch as graph. On the other hand, 
```sh
git reflog
```
will show detailed information of each commit. The most useful information is the commit hash ID.

Let's say you are sure that at commit A your code is working. In this way you may want to 
```sh
git checkout A
```
. In this way, you're cut out from the current branch history. Then you just need to checkout a new branch, update the code again and merge back to main branch. 

A more specific case is that you accidentally delete a file and commit. To reverse it you need to
```sh
git reset HEAD file_name.exd
git checkout -- file_name.exd
```
.
The first line unstages your change related to the file and the second one bring the file back from your branch history.