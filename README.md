# Git Notes

This is for working on a `main` branch on a fork.

Before starting, make sure you've set up your name and email in your git config:

```
git config --global user.email "email@example.com"
git config --global user.name "My Name"
```

1. On GitHub, fork the repo, using the button at the top right.

   ```
   https://github.com/upstream/repo.git ->  https://github.com/fork/repo.git
   ```

   The fork you've made will be your *origin* remote.

2. Get your fork locally:

   ```
   git clone https://github.com/fork/repo.git ~/path/repo
   cd ~/path/repo
   ```

   You'll also need access to the original repo for fetching changes. This is
   known as the *upstream* remote:

   ```
   git remote add upstream https://github.com/upstream/repo.git
   ```

   To prevent problems, add a `pushurl` line to the end of the `.git/config` file

   ```
   [remote "upstream"]
       url = https://github.com/upstream/repo.git
       fetch = +refs/heads/*:refs/remotes/upstream/*
       pushurl = no_push
   ```

3. Do some work

   ```
   git add -p
   git commit -m "My changes"
   ```

4. Rebase your changes on top of latest `main`

   ```
   git pull --rebase upstream main
   ```
   
   If you get any conflicts, don't panic, they probably are not too hard to resolve:
   
   ```
   git status
   # Resolve the problem by following the instructions in git status, usually:
   # e.g. vim package.json
   git add <conflicting file>
   git rebase --continue
   git status
   # Once everything is OK, review your log and see that your commits are at the top
   git log
   ```

   Since this takes a while, repeat the above again in case there are more changes.

5. Publish your rebased branch

   Then when you are sure everything is up to date:

   ```
   git push origin main
   ```

6. Create pull request on github

   Make any extra changes requested, and push them to the branch on origin that
   contains your pull request for that pull request to be updated.

7. The upstream maintainer will merge your pull request using the Rebase option.

   Extra info for the curious:
    
   * A pull request merge commit, is simply a commit with two parents and no patch that just tracks that the most recent commits on the two branches have been brought together. You can see this by running `git show <commit>` on a merge commit and a normal commit to see the differences.
   
   * A squash merge is like a big patch between the two branches, all as a single commit.
   
   * The rebase merge option on GitHub re-writes the committer on the commits.

8. Now you can pull in the upstream changes:

   ```
   git checkout main
   git pull --rebase upstream main
   # Deal with any conflicts using `git add` and `git rebase --continue` as before
   git status
   ```

   If everything goes well, there won't be any changes because your origin and
   local copy will both be in exactly the same state as upstream.

9. Every time you start working, repeat 8.

   This is so you get the latest changes before you start, which will reduce
   the chance of any conflicts at the end.

PRO tip: Use tab completion on everything to avoid mistakes, use `git status` a
lot, it gives you hints as to the commands you might want to use next.
