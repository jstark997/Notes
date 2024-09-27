# Repository

A repository or **repo** is a workspace that Git uses to track and manage files within a directory (folder).

Each repo is associated with a specific directory that can contain subdirectories (subfolders). However any files outside of the repo directory is not part of the repo.

**Note** - all git commands will only work within the directory of the repo.

## Status

The command **git status** returns information on the current status of a git repository or an error message if there is no repository in the directory that the command is executed in.

## Creating A Repo

To create a repo execute the command **git init** within the directory that the repo will reside. This will create a hidden subdirectory .git within the directory.

**Warning** - Do not run git init within a directory or subdirectory that is already part of a repo. This will confuse Git and will ultimately cause problems. If not sure whether a directory or subdirectory is part of an existing repo, then run git status to find out.

## .git Directory

The .git directory contains all the data Git needs to track and manage the files and subdirectories in the parent directory that is the repo.

## Deletion (Of Local Repo)

A (local) repo can be deleted by deleting the .git directory. 
