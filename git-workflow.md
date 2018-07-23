# Git Commands and Workflow for Github
The following is the git workflow for contributing to an open source project

## Adding Upstream and Branching

1. Start by forking the repo:
  - Navigate to repository you wish to fork
  - In the top-right corner of the page, click **Fork**
  - On your fork, click `Clone or Download` and grab the URL
  - Go to the command line and enter `git clone https://github.com/YOUR-USERNAME/REPOSITORY-NAME.git`
  - Then add the remote upstream: `git remote add upstream https://github.com/ORIGINAL-OWNER-USERNAME/REPOSITORY-NAME.git`
  - `git remote -v` to see if everything was added correctly
  - `git checkout -b feature-branch` to create a branch

Useful Git Commands:
`git branch` lists out all of your current branches
`git checkout <branch-name>` will allow you to switch branches

>

## MERGE WORKFLOW
1. On your working branch, make changes, and add/commit them.
2. Move back to master: `git checkout master`
3. Pull down any changes: `git pull upstream master`
    - OR if you didn't fork: `git pull origin master`
4. Move back to your working branch: `git checkout <branch-name>`
5. Merge in the new changes: `git merge

## REBASE WORKFLOW

### When you are ready to push your commits
1. Make sure all changed files are committed on feature-branch
2. Rebase on your feature-branch: `git pull --rebase upstream master`
3. Switch to your master branch: `git checkout master`
4. Rebase on your master: `git pull --rebase upstream master`
5. If there are changes then push changes to master: `git push origin master`
6. Go back to feature-branch: `git checkout feature-branch`
7. Push changes from feature-branch to github: `git push origin feature-branch`
8.  Submit pull-request on github
#### To update your origin master on github after your pr was accepted:
9. After pull-request is accepted, delete feature-branch from Github to clean up github history
10. On command-line switch back to master branch: `git checkout master`
11. Rebase from master branch: `git pull --rebase upstream master`
12. Push changes to your master: `git push origin master`
13. Delete feature-branch: `git branch -d feature-branch`
14. Create new branch: `git checkout -b new-feature`


### REBASE WORKFLOW TWO: If a somones PR has been merged on github and you want to rebase:
1. You would currently be working on your feature-branch. Do a git branch to check
2. Do a git status and commit any of your unstaged changes
3. Rebase from feature-branch: `git pull --rebase upstream master`
4. Switch to master branch and rebase: `git pull --rebase upstream master`
5. Push updated changes up to github origin-master: `git push origin master`
6. Move Back to feature-branch: `git checkout feature-branch`


* if you made a pull request and you need to make a change while your pr hasn’t been accepted yet, just commit the changes and push to your feature branch it will automatically be added to your open pull request.






## Merging an upstream repository into your fork


```bash
git pull https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git BRANCH_NAME
```


>[Merging vs rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

## Other

> To fetch a remote PR into your local repo,
```bash
git fetch origin pull/ID/head:BRANCHNAME
git fetch origin pull/4/head:login-form
```


