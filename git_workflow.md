# Git Workflow

This note is a quick introduction of how to do simple version control with Git. There are many code repository system. We use *GitHub* as an example.

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
git show commit_id
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
* sometimes you may want to use ```--squash``` option to combine your commits into one and then merge into main branch
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

## Deal with Commits

### Check Commit History

In this case, you've committed several times before you actually do any push or merge. however you find something wrong with the current version and wish to go back. The first thing you need to do is to heck your commit history.
```sh
git log --oneline --graph
```
will list all the commits from the most recent one to the oldest one. The option ```--oneline``` shows each commit with a line of abstraction while --graph shows each branch as graph. On the other hand, 
```sh
git reflog
```
will show detailed information of each commit. The most useful information is the commit hash ID.

### Checkout a Specific Commit

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

### Combine/Edit Several Recent Commits

Commit checkout will bring you out of the branch history, while sometimes you just want to stay in the branch history and do minor changes to commits. Here is an example. Let's say you want to combine 5 most recent commits. The logic is you reset to the 6th most recent commit, stage all changes and do a commit.

```sh
git reset most_recent_6th_commit_id # this is equivalent to git reset HEAD~5
git diff
git add .
git diff --staged
git commit
```
.

Each command line does the job to
* point to the 6th most recent commit as the current status
* check the difference
* stage all changes
* check the difference after staging
* do a commit.

In another situation, you may want to keep a couple of commits while edit others from commit A (keep A unchanged) to the most recent commit. Git provides
```sh
git rebase --interactive A
```
to interactively rebase and edit commits. During the process, you can press ```p``` to pick up and keep the commit, ```r``` to keep the commit but edit the commit message, ```e``` keep the commit and amend, ```s``` squash into previous commit and ```f``` squash into previous commit but discard log message.

### Revert a Specific Commit or Sequential Commits

Note that the command *revert* will not cancel any commit. It generates a new commit to offset the commit you want to revert.
```sh
git revert commit_id
```
If the commits you want to revert is sequential, 
```sh
git revert --no-commit commit_id_last_to_keep commit_id_recent_to_revert
```
.

If the commit is a true merge commit, use
```sh
git checkout branch_to_undo_merge
git log --graph --oneline
git revert --mainline 1 commit_id_for_merge
```
to
* checkout to the branch you want to undo merge
* check which branch, number from left to right, you want to keep (in most cases, the leftmost one should always be the base branch)
* revert the merge commit, option ```--mainline``` denotes which number of branch (leftmost = 1) you want to keep.

Otherwise, if you want to unmerge a branch right after the merge takes place, use
```sh
git reset --merge ORIG_HEAD
```
.

For more detailed illustration and systematic understanding of Git, readers may refer to [Git for Teams](http://gitforteams.com/) or [Git Documentation](https://git-scm.com/documentation). 
