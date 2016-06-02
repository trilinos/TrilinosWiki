The Trilinos github repository contains several long-lived branches:

* **develop**: The main development branch that Trilinos developers directly push commits to in order to add new features, fix bugs, etc.
  * _Who pulls from this branch?_
    * Close collaborators and customers that want to help test (not use) new features as they are being developed
    * Very close collaborators that want to have access to the very latest development version of Trilinos who don't mind dealing with more defects and increased instability
* **master**: A relatively stable version of Trilinos that is updated from the 'develop' branch when a specific set of builds for a specific set of packages have 100% passing tests
  * _Who pull from this branch?_
    * Close collaborators and users who want to access a very recent yet fairly stable version of Trilinos
* **trilinos-release-X-Y-branch**: Branch for the maintenance of the set of Trilinos X.Y.Z releases.  Official releases of Trilinos are tagged on this branch using the tag name **trilinos-release-X-Y-Z**
  * _Who pulls from this branch?_
    * External users that need named and supported versions of Trilinos with minimal-change patch releases for bug fixes (Note, users only should access tagged releases `trilinos-release-X-Y-Z` and not just pull from the tip of a release branch).

The following links provide information on Trilinos version control processes and workflows.

* See [How to Do Version Control with Git in Your CSE Project](http://ideas-productivity.org/wordpress/wp-content/uploads/2015/04/IDEAS-VCHowToVersionControlwithGit-V0.2.pdf) (an [IDEAS Productivity "How To" Document](https://ideas-productivity.org/resources/howtos/)).
* Do an [initial setup to use git](https://github.com/trilinos/Trilinos/wiki/VC-|-Initial-Git-Setup) on any new machine.
* Use the [simple centralized workflow](https://github.com/trilinos/Trilinos/wiki/VC-|-Simple-Centralized-Workflow) to integrate and push changes to a shared remote tracking branch (i.e. the 'develop' branch).
* For typical development, most developers will checkout, make changes, and push Trilinos changes to the ['develop' branch](https://github.com/trilinos/Trilinos/wiki/VC-|-'develop'-'master'-workflow).
* To fix defects on a release branch, use the [release branch merge workflow](???).
* Use the tools gitdist and checkin-test.py to help work with [Trilinos and extra repositories](???) (e.g. [inserted packages](https://docs.google.com/document/d/1fLSz7FM8hzmIfr84jQ9B9-C7eXhdLBu0aUyXfKS3XCU/edit#bookmark=id.nnw9n1fjkn34) like Sundance, CTrilinos, ForTrilinos, Mesquite and MOOCHO as well as [extra packages](https://docs.google.com/document/d/1fLSz7FM8hzmIfr84jQ9B9-C7eXhdLBu0aUyXfKS3XCU/edit#bookmark=id.x7n2akjjfrim) in repos like preCopyrightTrilinos).