# Policies for updating the public repository

## Introduction 

Updating the public repository currently involves a few manual steps. These steps have intentionally not been automated to provide an opportunity for a high-level review of the commits that have been made since the last public repository update. Commit authors are better judges of what should or should not go into a public repository, but this process does provide a second set of eyes.

Since updating the public repository requires checking for changes that should not go out in the public repository, we have included a discussion of how to modify the public repository filter in this policy. Because we also need to manage the contents of the distribution tarball, and it is efficient to scan for changes to the exclude lists for the tarball when scanning for changes to the public repository filter, we have also included below where to find the exclude lists for the tarball.

This process is in no way a substitute for R&A. Rather, the purpose of this process is to help identify things that may not already be covered under R&A, e.g. a new document. Trilinos developers adhere closely to the policies associated with committing to the repository, but due to the fact that commits to the Trilinos repository have historically not been made public as quickly (modifications were previously made public via a release tarball, and were reviewed using a similar process prior to release), and to the same extent as is the case now (release tarballs include only a snapshot of the code, not full history), we were advised to manually review new commits at a high-level prior to updating the public repository.

## Reviewing new commits

This is the most important part of the update process. Due diligence is required in checking the repository for changes that shouldn’t be released. In addition to checking for changes that shouldn't be released in the public repository, check for changes that shouldn't be part of the release tarball. There will be cases where some files should not be part of the release tarball, but can be part of the public repository. The opposite is not true. This process is not clear-cut, but some general guidelines should be applied. The following is a list of some of the most common kinds of changes that may need to be discussed with commit authors before updating the public repo. When in doubt, discuss a commit with its author prior to updating.

+ New documents, or other non-code files.

+ New files that appear to be experimental. (These files could be in a directory called experimental or have a name that suggests is it not a standard file.)

+ New files that appear to be experimental. (These files could be in a directory called experimental or have a name that suggests is it not a standard file.)

A good way to check for any added files would be to directly compare the publicTrilinos repository and the candidate public repository that is generated automatically each night. This can be done by running the compare_public_repos script in the public-repo directory in the cvsConversion repository. Running this script produces two files highlighting what is in the public_Trilinos candidate repository, but not yet in the publicTrilinos public repository: filelist.udiff, which is a list of files to be reviewed before updating the publicTrilinos public repository, and new_commits.txt, which is a list of commits that need to be approved before being added to the publicTrilinos repository. Reviewing these two files can help to identify situations that need to be examined more closely.

## Excluding files when necessary

When files or directories must be excluded from the public repository, and/or the distribution tarball, follow the below processes:

### Excluding files or directories from the public repository

To exclude files from the public repository, update the filter used to generate the candidate public repository, public_Trilinos. **Fill in how to update filter and regenerate candidate repo.** Keep in mind that it will break history for public repository users to either add files or directories to the filter list that are already included in the public repository, or to stop filtering files or directories that are currently being filtered out of the public repository. These actions should be taken only when necessary and should be announced to public repository users.

### Excluding files or directories from the distribution tarball

The exclusion list for individual packages can be found in CMakeLists.txt in the top-level packages directory, in TRIBITS_EXCLUDE_FILES. If this variable does not already exist in the file, simply add it, using another package as a model if necessary.

The exclusion list for the part of Trilinos not specific to any package, and for excluding entire packages can be found in Trilinos/cmake/CallbackDefinePackaging.cmake in CPACK_SOURCE_IGNORE_FILES.

## Updating the public repository

The cvsConversion/public-repo/update-repo.sh script handles all of the actual update process including updating the server information for the repository and generating a file with the dates of when the update was run and of the most recent commit on the master branch. There is currently a copy of this script in /var/www/html/trilinos/repositories. It should be verified that the script is up-to-date, though it shouldn’t change very often.

Below are the steps that should be taken to update the public repository.

1. Check the public repository candidate that is automatically generated each night for changes that might prevent us from updating the repo. See the section “Reviewing new commits” for some guidance on how to do this. The public repo candidate is currently located at /home/bmpersc/repositories/public_Trilinos.git. (Note: One useful way to inspect for new files is to run “find .” in your clones of both the publicTrilinos repo and of the candidate public repo, piping the output of find to a file. Then use diff on those files to see any changes. Occasionally to do this you might need to sort the output of find as the files might be reported in a different order.)

2. The publicTrilinos repository is in /var/www/html/trilinos/repositories on software.sandia.gov. If the review for new commits shows that the public repository can be updated, run the update-repo.sh script from the top level of the publicTrilinos repository without any arguments, e.g. ../update-repo.sh.

3. Verify that the update_date.txt file was updated correctly.
