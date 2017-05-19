# TrilinosWiki

This is git repository to facilitate changes to the Trilinos wiki https://github.com/trilinos/Trilinos.wiki.git using pull requests.

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
