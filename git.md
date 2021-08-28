# Git Workflow

## Git commit message rules

1. Separate subject from body with a blank line
2. Limit the subject line to 50 characters
3. Capitalize the subject line
4. Do not end the subject line with a period
5. Use the imperative mood in the subject line
6. Wrap the body at 72 characters
7. Use the body to explain what and why vs. how

Read more: https://chris.beams.io/posts/git-commit

## What is `git merge`?

- `git merge` is one of two ways of applying changes of one branch onto another
- used to combine two branches with a "merge commit", which symbolizes the joining of the two branches
- most straightforward way of combining two branches
- See [Git Merge | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/using-branches/git-merge) for more info about `git merge`

## What is `git rebase`?

- `git rebase` is the second way to integrate changes of one branch onto another branch
- allows us to preserve the original commits of a branch
- "plucks" commits from a branch and attaches them one-by-one to a different commit
- enables a much cleaner project history
- See [git rebase | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase) for more info about `git rebase`

## `git merge` vs `git rebase`

- `git merge` will create a **"merge commit"** onto the target branch to represent the commits brought by the incoming branch, the "merge commit" will be the contract between the target branch and the incoming branch of the changes that will be applied
- `git rebase` preserves the original commits on the target branch, and instead applies the individual commits of the incoming branch one-by-one onto the target branch
- See [Merging vs. Rebasing | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/merging-vs-rebasing) for more info about `git merge` vs `git rebase`

## What is `git pull --rebase`?

- `git pull` will run `git fetch` and incorporate changes to your branch using `git merge`
- `git pull --rebase` runs `git rebase` instead of `git merge` when running `git pull`
- `git pull --rebase` will fetch and replay the remote branch onto the local branch using `rebase`, meaning it will preserve the original commits and will not create a `merge` commit for the changes that you have pulled.
- it would be as if you have added your commits directly on top of the branch you are rebasing to, without the extra merge commit that would have been generated if `git pull` was used

## What is the Golden Rule of Rebasing?

> The golden rule of git rebase is to never use it on public branches.

`git rebase` rewrites the project history by moving all of the commits of one branch onto another branch. This means that you should only use `rebase` on your forks, and not on the main repo's branches, where multiple developers are basing their own branches from.

## Our workflow

Technically, we use `merge` for the main repository branches, but utilize `rebase` for the forks. The main repository should contain merge commits plus the individual commits for every merged pull request. The local branches in individual forks should be rebased onto the main repo's base branch before creating a pull request.

- Create a fork of the main repository
- Clone your fork of the repository locally (`origin`)
- Create a local branch (feature/fix/chore/etc.), ideally from the main repo's (`upstream`) base branch
- Add changes and commits to your branch
- Use `git pull --rebase upstream master` on your local branch before creating a pull request to make sure that the incoming branch is updated
- Push your branch to your remote and create a pull request to the target branch on Github
- Upon approval of your pull request, admin should create a "merge commit" when merging, instead of squashing or rebasing

### On using `git pull --rebase` on local branches before every pull request

- allows local branches to always be updated from the main repo's base branch
- allows handling conflicts more gracefully
- prevents unmergeable pull requests due to conflicts

### On creating `merge` commits when merging pull requests

- adds visual marks at the end of every pull request
- makes the branches & merge history of the whole repository easily traceable and visualizable
