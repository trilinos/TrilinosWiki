# TrilinosWiki

This is git repository to facilitate changes to the Trilinos wiki https://github.com/trilinos/Trilinos.wiki.git using pull requests.

## Suggesting Contributions to Trilinos Wiki as a PR

To use this to suggest changes to the Trilinos wiki, do the following:

**1) Create a fork of this github Wiki repo**

Click the above "Fork" button to create the GitHub repo:

* `https://github.com/<yourgithubid>/TrilinoWiki/`

**2) Set up your local GitHub Wiki repo**

```
$ git clone git@github.com:trilinos/Trilinos.wiki.git
$ cd Trilinos.wiki/
$ git remote add wiki-pr git@github.com:trilinos/TrilinosWiki.git
$ git remote add my-wiki git@github.com:<yourgithubid>/TrilinosWiki.git
```

**3) Create a new topic branch for your proposed changes**

```
$ cd Trilinos.wiki/
$ git fetch origin
$ git checkout -b my-changes origin/master
$ git push -u my-wiki my-changes:my-changes
```

**4) Create your proposed changes in MarkDown using the GitHub interface**

Go to your GitHub repo:

* `https://github.com/<yourgithubid>/TrilinoWiki/`

and then edit the `*.md` files in the new branch `my-changes` right there and use the preview feature to inspect your changes as you make them.

Then create commits in that GitHub site as you like on your branch `my-changes`.

**5) Post your proposed changes as a pull request**

Go to the GitHub repo:

* https://github.com/trilinos/TrilinoWiki/

and create a new pull reqeust of your new branch `my-changes` against the `master` branch of this repo.  Make sure to mention @trilinos/framework.

NOTE: The `master` branch in the repo https://github.com/trilinos/TrilinoWiki/ needs to be kept up-to-date in order for the PR to show the right set of new commits.  This may need to be a manual process once the PR is reviewed by Trilinos Framework staff.

## Reviewing and merging PR branch to Trilinos Wiki

Once some suggests change to the Trilinos wiki is provided as a PR in this github repo as described above, then a Trilinos developer with push access to the Trilinos wiki repo needs to review the PR, and if acepted, merge it to the Trilinos wiki and close the PR.  This is described in the following process.

To get setup for this, first the Trilinos developer who will merge the branch creates a local git repo for the Trilinos wiki:

```
$ git clone git@github.com:trilinos/Trilinos.wiki.git
$ cd Trilinos.wiki/   # 'origin' = git@github.com:trilinos/Trilinos.wiki.git
[(master)]$   # Tracking origin/master
```

**1) Review the PR**

Review the PR by inspecting the updated files and provding comments (just like any GitHub repo and PR).  The contributor can then make changes based on feedback.  This can happen iteratively until the changes are acceptable for merging.

NOTE: The `master` branch may need to be manually updated as follows:

```
[(master)]$ git remote add github-pr git@github.com:trilinos/TrilinosWiki.git
[(master)]$ git fetch github-pr
[(master)]$ git checkout github-pr-master github-pr/master
[(github-pr-master)]$ git fetch origin
[(github-pr-master)]$ git merge origin/master
[(github-pr-master)]$ git push   # to github-pr/master
```

That will ensure that the right set of commits are listed in the PR.

**2) Merge the updated PR branch**

Create a remote to the contributor's (GitHub ID `<contrib-github-id>`) fork of this TrilinosWiki github repo:

```
[(master)]$ git remote add <contrib-github-id> git@github.com:<contrib-github-id>:TrilinosWiki.git
[(master)]$ git fetch <contrib-github-id>
```

Merge in the contributor's branch `<contrib-branch>` that was posted as part of the PR:

```
[(master)]$ git pull    # from origin/master
[(master)]$ git fetch <contrib-github-id>
[(master)]$ $ git merge <contrib-github-id>/<contrib-branch>
```

Review the changes just to make sure:

```
[(master)]$ git log --name-status HEAD ^@{u}
[(master)]$ git diff HEAD ^@{u}
```

Push the changes to the wiki:

```
[(master)]$ push   # to origin/master
```

**3) Comment and close the PR Issue**

Once the branch is merged, the Trilinos developer will need to manually comment on the merged branch and manually close the PR.
