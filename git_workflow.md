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

If we want to update the project, such as adding new feature or fixing a bug, the better practice would be create a new local branch and merge to master at last.

