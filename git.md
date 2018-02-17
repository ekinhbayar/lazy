## Git
Find out what's the defined global gitignore file

`git config --get core.excludesfile`

Add a new remote

`git remote add upstream https://github.com/user/repo.git`

Rename a remote

`git remote rename ustream upstream`

Remove a remote

`git remote rm upstream`

Inspect a remote

`git remote show origin`

Revert your local bad commit (restore everything back to the way it was prior to the last commit)


`git reset HEAD^`

use `--soft` if you want to keep your changes (last `git add` stays)
