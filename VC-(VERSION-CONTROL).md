The following links provide information on Trilinos version control processes and workflows.

* See [How to Do Version Control with Git in Your CSE Project](http://ideas-productivity.org/wordpress/wp-content/uploads/2015/04/IDEAS-VCHowToVersionControlwithGit-V0.2.pdf), an [IDEAS Productivity "How To" Document](https://ideas-productivity.org/resources/howtos/).
* Do an [initial setup to use git](???) on any new machine.
* Use the [simple centralized workflow](https://github.com/trilinos/Trilinos/wiki/VC-|-Simple-Centralized-Workflow) to integrate and push changes to a shared remote tracking branch (i.e. the 'develop' branch)
* For typical development, most developers will checkout, make changes, and push Trilinos changes to the ['develop' branch](https://github.com/trilinos/Trilinos/wiki/VC-|-'develop'-'master'-workflow).
* To fix defects on a release branch, use the [release branch merge workflow](???).
* Use the tools gitdist and checkin-test.py to help work with [Trilinos and extra repositories](???) (e.g. [inserted packages](https://docs.google.com/document/d/1fLSz7FM8hzmIfr84jQ9B9-C7eXhdLBu0aUyXfKS3XCU/edit#bookmark=id.nnw9n1fjkn34) like Sundance, CTrilinos, ForTrilinos, Mesquite and MOOCHO as well as [extra packages](https://docs.google.com/document/d/1fLSz7FM8hzmIfr84jQ9B9-C7eXhdLBu0aUyXfKS3XCU/edit#bookmark=id.x7n2akjjfrim) in repos like preCopyrightTrilinos).