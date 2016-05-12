The Trilinos project uses a long-lived branch called 'develop' to conduct basic development.  All developers directly pull from and push to the 'develop' branch.  Then, the 'master' branch is updated from the 'develop' branch when it passes a specific set of builds for a specific set of packages (the set will change as needed).  The motivation for the usage of a 'develop' branch and the full set of mechanics and implications are described in [Addition of a 'develop' branch](https://docs.google.com/document/d/1uVQYI2cmNx09fDkHDA136yqDTqayhxqfvjFiuUue7wo/edit#heading=h.u2ougk1wk7ph).

Modifying the workflow from the [Simple Centralized Workflow](https://github.com/trilinos/Trilinos/wiki/VC-|-Simple-Centralized-Workflow) is easy.  Just get on a local 'develop' tracking branch.  If the local 'develop' tracking branch has not already been created, then create it using:

```
$ cd Trilinos/
[ (master)]$ git fetch origin
[ (master)]$ git checkout --track origin/develop
[(develop)]$ 
```

If the local 'develop' tracking branch is already created (can be seen by running `git branch`), then just check out that branch using:

```
[ (master)]$ git checkout develop
[(develop)]$ 
```

Once on the local 'develop' branch which is tracking the remote 'origin/develop' branch, one simply uses the [Simple Centralized Workflow](https://github.com/trilinos/Trilinos/wiki/VC-|-Simple-Centralized-Workflow) using raw `git pull`, `git commit` and `git push` commands, i.e.:

```
[(develop)]$ git pull                               # from origin/develp
[(develop)]$ emacs <files>                          # or using other editor
[(develop)]$ git commit -a
[(develop)]$ git status
[(develop)]$ git log --name-status @{u}..HEAD
[(develop)]$ git pull                               # from origin/develop
... Test changes (e.g. using checkin-test.py) ...
[(develop)]$ git pull --rebase                      # from origin/develop
[(develop)]$ git push                               # to origin/develop
```

NOTE: The checkin-test.py script performs the steps from the second `git pull` after the local modifications are commited to the final `git push` robustly in a single command.
