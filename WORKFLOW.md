# Workflow

* First do the [initial setup to use git with Trilinos](???)
* Use the [simple CI workflow](???) to integrate and push changes to a shared remote tracking branch (i.e. the 'develop' branch)
* For typical development, checkout, make changes, and push changes changes for Trilinos to the 'develop' branch as part of the ['develop'/'master' branch workflow](???)
* To fix defects on a release branch, commit the bug-fix directly to the release branch and then merge to the 'develop' branch using the [release merge workflow](???).
* Use the gitdist and checkin-test.py tools to help work with [Trilinos and extra repositories](???) (e.g. ???inserted packages??? like Sundance, CTrilinos, ForTrilinos, MOOCHO, and preCoprightTrilinos).
