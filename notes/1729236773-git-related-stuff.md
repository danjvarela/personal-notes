---
id: 1729236773-git-related-stuff
aliases:
  - git-related-stuff
tags:
  - git
---

# git-related-stuff

## Optimized git workflow using git worktrees

My previous git workflow is to:

1. Clone a repo.
2. Create worktrees inside that repo.

The problem with this approach is you have a bunch of files inside the directory where your worktrees are located since it is checked-out to a branch (`main` or `master` by default).
[This Stackoverlow answer](https://stackoverflow.com/a/54408181/19140417) details a more optimized approach.

1. Clone the repo.
2. Go inside the directory where you cloned it.
3. Execute this command:

   ```sh
   git checkout $(git commit-tree $(git hash-object -t tree /dev/null) < /dev/null)
   ```

   To quote the Stackoverflow answer's author, @torek:

   > The git hash-object -t tree /dev/null produces the hash ID of the empty tree that already exists in every repository. The git commit-tree makes a commit to wrap that empty tree—there isn't one, so we must make one to check it out—and prints out the hash ID of this new commit, and the git checkout checks that out as a detached HEAD. The effect is to empty out our index and work-tree, so that the only thing in the repository work-tree is the .git directory. The empty commit we made is on no branch and has no parent commit (it's a lone root commit) that we will never push anywhere.

Now I have a directory that only contains .git, where I can create my worktrees.
