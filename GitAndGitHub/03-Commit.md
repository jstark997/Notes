# Commit

A **commit** is a reference or checkpoint to a specific state or version of the repo that includes the most recent changes. In other words it is a snapshot of the repo at a particular point in time. 

Best practice is to include a message that describes the changes included in the commit. The Git docs recommend that messages be in present tense and imperative style.

A commit will often group together changes to serveral files. Best practice is to organize related changes into a single (**atomic**) commit. This makes it easier to revert changes and to review them.

## Basic Workflow

* Make changes - Add, edit, delete files.
* Add changes - Group together related changes.
* Commit - Add grouped changes to a snapshot of the repo.

## Git Logical Structure

* Working Directory - the directory that the repo is associated with and that contains all the files for a project.
* Staging Area - a logical location where files are grouped together in preparation to be commited to the repo.
* Repository - Instantiated by the .git directory which contains all the data about the repo including all the snapshots (commits) created.

## Add Command

The **git add** command adds changes to the staging area as a group in preparation to be committed to the repo.

To add specific files list the files after the add command separated by a space.

```sh
git add file1 file2
```

To add all the files in a directory specify the current directory with a '.'

```sh
git add .
```

To remove a file from the staging area (unstage) type the following specifying the name of the file to remove:

```sh
git rm --cached file1
```

## Commit Command

The **git commit** command will commit all the files in the staging area to the repo. 

**Caution** - Calling just 'git commit' will bring up the default text editor to ask you to enter a message for the commit.

To avoid having to use the default text editor use the '-m' flag to directly specify the commit message when calling the command as shown below.

```sh
git commit -m "Message describing the commit."
```

## Status Command

Git tracks any changes to the working directory, whether those are new files, modified files (files that have already been committed but changed since the last commit), or deleted files.

The status of this tracking can be revealed as follows:

```sh
git status
```

## Log Command

To retrieve a log of all of the commits use the log command as follows:

```sh
git log
```

The log will show the hash, the author, the date and the message for each commit.

However, a convenient abbreviated version of the log that only shows the beginning of the hash and one line of the message is as follows:

```sh
git log --oneline
```

## Amending Commits

The previous commit can be amended in order to fix any mistakes by using the 'amend' flag.

```sh
git commit --amend
```

This works for amending a commit for a file(s) that was forgotten or to edit the message. To add files that should have been part of the previous commit, need to call 'git add' to stage them and then called 'git commit --amend'. This will display the previous commit message in the default text editor, which can then be edited.




