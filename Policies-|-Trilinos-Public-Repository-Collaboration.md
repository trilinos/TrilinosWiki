# Trilinos Public Repository Collaboration

### Introduction 

The Public Repository is available to make it easier to interact with collaborators and users who do not have access to the main Trilinos repository. General information about the public repository can be found here.

There are several policies and processes associated with the public repository that are focused on maintaining repository stability and making the collaboration process easy for everyone involved.

### Applying Patches

Unless another process has been agreed upon, public repository users are asked to submit patches using git format-patch. Applying patches created in this format is easy, simply use 

    git am patch_file

for each patch_file in the format ####-commit_summary.patch, where #### is the number of the order in which the patches should be applied.

After one or more patches have been tested as described below, the patches can be pushed to the main Trilinos repository. Note that if additional changes need to be made prior to pushing a patch, it is best to commit the changes as a separate commit, rather than modifying one of the submitted commits. The reason is that the user who submitted the patch files will end up with conflicts if a commit is modified, but will be able to rebase successfully if patches are pushed without modification. The new commits can be pushed at the same time as the original patch file commits to prevent repository instability.

Developers who push changes submitted by contributors are responsible for the contents of the commits. Remember that contributors may have a narrow scope in their work, lack of access to platforms for testing and/or may not adhere to sufficient levels of software quality practices.

### Testing Patches

Submitted format-patch files should be thoroughly tested before pushing changes to the main Trilinos repository. In addition to reviewing the general appropriateness of the patch and the line-by-line changes, a test (or tests) in the test suite should exercise the new or modified feature. Ideally, this test should be submitted along with the source code modifications as a patch. If not, it is appropriate to ask the submitter for a test, or, in some cases, it makes sense for the developer applying the patch to add tests.

After specific tests have been added and run for the feature, the Trilinos test suite for the modified package(s) and** all forward packages** should be run on one or more platforms. It is recommended to do this through the [Trilinos checkin test script](https://github.com/trilinos/Trilinos/wiki/Safe-Checkin-Testing).

After pushing, please remember to take note of any continuous testing failures over the next two hours, and nightly failures the next morning, as for any other source code change.

### Requesting an Update to the Public Repository

At this time we are not able to update the public repository on an automated, continuous basis. We are planning to keep the repository reasonably up-to-date with current development, although "reasonable" in this context is still being defined. If you are working closely with someone on a patch, or have some other need to update the public repository quickly, please send a request to the [Trilinos Framework mail list](https://github.com/trilinos/Trilinos/wiki/Mail-Lists).
