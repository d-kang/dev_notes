- [ ] what is ssh git ?
- [x] whats the diff between `$ git push` and `$ git push origin master` ?
- [ ] whats the diff between `$ git merge` and `$ git fetch`?

## What is git clone doing?
1. Create a repository on github
2. `$ mkdir new-folder`
3. `$ cd new-folder`
4. `#git init`
```
# .git/config
[core]
repositoryformatversion = 0
filemode = true
bare = false
logallrefupdates = true
ignorecase = true
precomposeunicode = true
```
6. `$ git remote -v` will show nothing
7. `$ git branch` will show nothing
8. `$ touch index.html` create a file
9. `$ git add index.html` add the file
10. `$ git commit -m "first commit"` commit the file
11. `$ git branch` now you have a master branch
12. now if we try `$ git push origin master`
```
# .git/config
fatal: 'origin' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```
14. `$ git remote add origin https://github.com/YOUR-HANDLE/REPO-NAME.git`
```
# .git/config
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
[remote "origin"]
	url = https://github.com/d-kang/test.git
	fetch = +refs/heads/*:refs/remotes/origin/*
```
15. `$ git remote -v` will show origin
16. `$ touch index.html`
17. `$ echo "<h1>Hello World</h1>" >> index.html`
18. `$ git push origin master` should now work


19. BUT `$ git push` doesn't work
```bash
❯ git push
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master
```
18. `$ git push --set-upstream origin master`
```
# .git/config
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
[remote "origin"]
	url = https://github.com/d-kang/test.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
```
19. now `$ git push` works

## git merge or git pull
21. `$ git branch --set-upstream-to=origin/<branch> test-branch-1`
```
# .git/config
[branch "test-branch-1"]
	remote = origin
	merge = refs/heads/master
```

> `$ git clone https://github.com/YOUR-HANDLE/REPO-NAME.git`
> mkdir REPO-NAME
> git init
> git remote add origin https://github.com/YOUR-HANDLE/REPO-NAME.git
> git push --set-upstream origin master




[Why do I have to “git push --set-upstream origin <branch>”?
](https://stackoverflow.com/questions/37770467/why-do-i-have-to-git-push-set-upstream-origin-branch)


[Difference between git merge and git fetch?](https://stackoverflow.com/questions/292357/what-is-the-difference-between-git-pull-and-git-fetch)
[Why git fetch doesn't download anything?](https://stackoverflow.com/questions/41779529/why-git-fetch-doesnt-download-anything)

[where does the fetched commits go when we git fetch? - yt video](https://www.youtube.com/watch?v=EnCe89ioCZQ)



`$ git fetch origin`
`$ git merge origin/master`


if two people edit the same line and they differ, merge conflict
both forms cannot coexist at the same time
think of a deck of cards, you cant push one card into another, its physically impossible. but you can stack them on top or below each other.
thats what resolving merge conflicts basically is

however with git pull, you are rewriting hist. so sometimes you can get into a paradox.


if two peopple edit the same line to the exact same thing, no merge conflict