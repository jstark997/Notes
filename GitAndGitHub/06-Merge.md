# Merge

Incorporating the changes of one branch into another is called **merging**.

## Typical Workflow

The master or main branch is often treated as the 'source of truth' or the most stable release or the release that is approved to be made available to the public or to be part of running production code.

Therefore, a typical workflow to add new functionality or a new feature, is to create a new branch for that feature and when that feature is completed and tested to merge that branch back into the master branch.

## Merge Command

The merge command merges the named branch into the branch that HEAD is currenlty pointing to. Therefore, you should switch to the branch you want to merge into (changing HEAD to point to that branch) before calling the merge command.

Below is an example of using the merge command to merge a branch called 'new-feature' into the master branch:

```sh
git switch master
git merge new-feature
```

## Fast Forward Merge

A fast forward merge is a special merge case where the pointer to the tip of the branch that is merged into is simply moved forward to where the pointer of the tip of the branch that is merged from is. This will only be the case if the branch that is being merged from has all of the commits of the branch to be merged into. In other words the commits of the branch to be merged into are all chronologically behind the commits of the branch to be merged from.


## Merge Commit

If the commits of the branch to be merged into are not all chronologically behind the commits of the branch to be merged from, but none of the overlapping commits conflict with each other, then Git can merge the branches automatically in what is called a 'merge commit'. Git will create a new commit for the merged branches that will be ahead of the tip of both branches. The command for a merge commit is still 'git merge'. For the new commit Git will prompt for a commit message with a default message.

## Merge With Conflicts

Merge conflicts can arise if the changes in one branch cannot be accepted without affecting the changes in the other branch to merge. For example, if a file in one branch is deleted but in the other branch it is changed, or if the same line in the same file is changed differently in the two branches to merge. In these cases Git cannot automatically merge the branches, but will require the user to specify which changes to keep in the merge.

Steps to resolve merge conflicts:

1. Execute merge command
2. Git reports that there are conflicts.
3. For each file that has a conflict (which Git will inidicate with conflict markers in the file), edit the file to resolve the conflict (and remove the conflict markers).
4. Add (stage) the edited files that resovle the conflicts.
5. Commit the changes.





