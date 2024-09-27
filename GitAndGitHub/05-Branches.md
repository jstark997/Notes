# Branches

A branch is a seperate context for a project. It contains commits up to the point that the branch was created. After that point the commits to the branch are separate from any other branch and do not affect them (unitl the branch is merged with another branch). A branch can be thought of as an alternative timeline for the project. They allow work on a project to be done in parallel.

## Master Branch

When a repo is created a default branch, called **master**, is also created. This is just the default branch and has no special significance within Git. However, it is common for the master branch to be considered the 'official' branch. That is from the master branch other branches are created to add features or fix bugs and when those branches are completed, they are merged back into the master branch, which serves as the version of the project that is acceptable to be released to end users.

**Note** - In 2020 GitHub changed the default branch name for repos created in GitHub to main.

## HEAD

Is a pointer to the most recent commit in the branch that is currently active. If you are working in the master branch, then HEAD will point to the most recent commit in that branch. However, if you switch to a differenct branch, then HEAD will point to the most recent commmit in that branch.

## Viewing Branches

To view all the branches in a repo execute the following:

```sh
git branch
```

The branch that is currently active will have an asterisk next to its name. To get out of the list type 'q'.

## Creating A Branch

To create a branch execute 'git branch' followed by the branch name.

```sh
git branch new-feature
```

This will create a branch but will not switch to it. That is HEAD will remain where it was.

**Important** - The branch created will be created based on where the HEAD is at the time of the call to 'git branch'. So be mindful of which branch HEAD is pointing to when creating a new branch.


## Switching To A Branch

To switch to a branch (change the branch HEAD is pointing to) there are two commands: 'switch' and 'checkout'. The 'switch' command was more recently added to Git.

```sh
git switch new-feature
```

Or

```sh
git checkout new-feature
```

The 'checkout' command has functionality beyond just switching branches (which is way the 'switch' command was created to reduce confusion).

Also, the 'switch' command has a flag that allows the creation of a new branch and switching to it simultaneously.

```sh
git switch -c other-new-feature
```

The above will create the branch 'other-new-feature' and move HEAD to point to it.

Finally, the 'checkout' command also allows the simultaneous creation and switching of a branch.

```sh
git checkout -b another-new-feature
```

**Important** - If there are unstaged changes to a file that was previously committed and you attempt to switch branches without committing them, Git will complain that the file is in conflict and that you need to either commit it or stash it. However, if the unstaged file is a new file that does not exist in any other branch, then switching branches is allowed and the file will follow, as an untracked file, to the branch that was switch to.

## Deleting A Brach

To delete that has been fully merged use the 'git branch' command as follows:

```sh
git branch -d completed-feature
```

**Note** - Deleting a branch cannot occur within the branch, it must be done from some other branch.

To delete a branch that has not be fully merge execute the following:

```sh
git branch -D not-fully-merged-feature
```

## Renaming A Branch

To rename a branch switch to the branch and execute the 'git branch' command as follows:

```sh
git switch old-branch-name
git branch -m new-branch-name
```

**Note** - The '-m' flag stands for 'move' and recalls how files are renamed in Linux using the 'mv' command.






