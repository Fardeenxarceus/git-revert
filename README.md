This repo is about learning git revert command
"git revert [commit id]" help us to restore our code if it's pushed or committed

we can find our commit id on the code page of repositories

There is one more command like git revert [commit id] but it's a little bit different than it is.
The command is "git reset [commit id]
It's different because when we use "git revert [commit id]" it restore the previous phase of repo but it still keep tracks of last commit whereas "git reset [commit id]" erase the commit we are trying to go back. it removes from commit history and we shouldn't use it when we are collaborating.
