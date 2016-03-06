A nice way to do backups of your work on branches without having to share them to the entire world is to create a private Trilinos git repo under your own bitbucket.com (BB) account.  You can use this git repo to back up branches you are working on without having to share them.  If you back up branches to your (public) GitHub fork of Trilinos, then everyone can see them.  If you push branches to a private BB repo, then no one can see them until they are ready.

BitBucket.com allows for an unlimited number of private git repos with some restrictions (e.g. can only give access to 5 other bitbucket accounts).

To set up a private bitbucket.com repo, do the following:

1. Create a new bitbucket.com account `<bb-account>` (if you don't already have one)
2. Register you SSH public key with your `<bb-account>` account
3. Create a new private empty BB repo `Trilinos` under your account `<bb-account>`
4. Create a remote for your private BB repo: `git remote add my-bb git@bitbucket.com:<bb-account>/Trilinos`
5. Push your local branch: `git push -u my-bb <my-branch>`
