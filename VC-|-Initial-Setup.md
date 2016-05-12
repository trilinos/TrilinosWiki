Before getting started using git with Trilinos (or any other project that uses git), one should perform a basic setup of git (see [How to Do Version Control with Git in Your CSE Project](http://ideas-productivity.org/wordpress/wp-content/uploads/2015/04/IDEAS-VCHowToVersionControlwithGit-V0.2.pdf), an [IDEAS Productivity "How To" Document](https://ideas-productivity.org/resources/howtos/)):

**1) Set up minimal Git settings for your account**, including “user.name,” “user.email,” “color.ui,” “push.default,” and “rerere.enabled”:

```
$ git config --global user.name "First M. Last"
$ git config --global user.email "youremail@someurl.com"
$ git config --global color.ui true         # Use color in git output to terminal
$ git config --global push.default simple   # or 'tracking' with older versions of git
$ git config --global rerere.enabled 1      # auto resolve of same conflicts on rebase!
```

**2) Install Git helper scripts locally** for the Git shell prompt [git-prompt.sh](https://raw.github.com/git/git/master/contrib/completion/git-prompt.sh) and Git tab completion [git-completion.bash](https://raw.github.com/git/git/master/contrib/completion/git-completion.bash), and add them to your shell, for example, setting the following in your `~/.bash_profile` file:

```
source ~/git-prompt.sh
source ~/git-completion.bash
PS1='[\u@\h \W$(__git_ps1 " (%s)")]\$ '
```

These make using git so much easier!

**3) If [developing on multiple git repositories](???) install and setup the usage of `gitdist`**:

**3.a) Install gitdist into your env**, for example, after clonging Trilinos, set in your `~/.bash_profile` file:

```
alias gitdist=$TRILINOS_HOME/cmake/tribits/python_utils/gitdist
```

**3.b) Set up useful `gitdist` alias**, for example, in your `~/.bash_profile` file:

```
alias gitdist-status="gitdist dist-repo-status"
alias gitdist-mod="gitdist --dist-mod-only"`
alias gitdist-mod-status="gitdist dist-repo-status --dist-mod-only"
```
