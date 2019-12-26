# git-snips
Git CLI snippets


## Setup config (global,github.com)

(optionally substitute --global for --local)
```
git config --global user.name "My Name"
git config --global user.email "username@username.users.noreply.github.com"
```

## Update wd to remote, nuking any local changes
```
git fetch origin
git reset --hard origin/master
```
[ref](https://stackoverflow.com/questions/6284809/how-can-i-pull-from-remote-git-repository-and-override-the-changes-in-my-local-r)

## Update remote, nuking any remote changes
```
git checkout --orphan latest_branch
git add -A
git commit -am "commit message"
git branch -D master
git branch -m master
git push -f origin master
```
[ref](https://stackoverflow.com/questions/39389380/how-to-overwrite-remote-branch-with-different-local-branch-with-git)


## Checkout remote branch (=>1.6.6)
```
git fetch
git checkout -b test <name of remote>/test
```

[ref](https://stackoverflow.com/questions/1783405/how-do-i-check-out-a-remote-git-branch)


## Create remote branch
```
git checkout -b <branch-name> # Create a new branch and check it out
git push -u <remote-name> <branch-name>
```
`or`
```
git push -u <remote-name> <local-branch-name>:<remote-branch-name>
```

#### Set upstream. I beleive `-u` accomplishes this
```
git push --set-upstream <remote-name> <local-branch-name>
```


[ref](https://stackoverflow.com/questions/1519006/how-do-you-create-a-remote-git-branch)


## Add existing repo to a remote repository
```
git remote add origin <remote repository URL>
```
###### Sets the new remote
```
git remote -v
```
###### Verifies the new remote URL
```
git push -u origin master --force
```
###### Pushes the changes in your local repository up to the remote repository you specified as the origin

[ref](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line)


## Changing author of all commits

```
git clone --bare https://github.com/user/repo.git
cd repo.git
```
```
#!/bin/sh

git filter-branch --env-filter '

OLD_EMAIL="your-old-email@example.com"
CORRECT_NAME="Your Correct Name"
CORRECT_EMAIL="your-correct-email@example.com"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags

```

```
git push --force --tags origin 'refs/heads/*'
cd ..
rm -rf repo.git
```

[ref](https://help.github.com/en/github/using-git/changing-author-info)
