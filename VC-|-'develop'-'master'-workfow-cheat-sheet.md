First time in each local git repo:

``` 
$ cd Trilinos/
[(master)]$ git fetch
[(master)]$ git checkout --track origin/develop
[(develop)]$ 
```

Otherwise, do:

```
[(master)]$ git checkout develop
[(develop)]$ 
```
 
Once on local 'develop' tracking branch, then just do the [centralized workflow](VC-%7C-Simple-Centralized-Workflow).

See more details [here](VC-%7C-'develop'-'master'-workflow).

**WARNING:** Don't make the mistake the following mistake:

```
$ git checkout -b develop
```

That is **NOT** a tracking branch and raw `git pull` and `git push` will not work!
