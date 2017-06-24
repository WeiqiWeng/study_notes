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

Make sure the new feature passes all the tests and works as expected. Then we can merge to branch *master* and put to production.
```sh
git chechout master
git merge update_the_project
git branch --delete update_the_project
```
Each shell does the corresponding work:
* switch to branch *master*
* merge *master* and *update_the_project*
* delete the branch for sanity.

If we have serveral changes committed in the sub-branch and we want to merge these commits with the commits in branch *master* at once, we can use
```sh
git chechout master
git merge update_the_project --squash
```
.

We can delete the branch as illustrated above.


