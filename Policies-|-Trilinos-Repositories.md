# Policies for and List of Trilinos Repositories

## Introduction

When we refer to the "Trilinos repository", we are referring to the primary Trilinos git repository located on [this GitHub repository](https://github.com/trilinos/Trilinos). However, there are a large number of repositories associated with the Trilinos Project. Below is a list of repositories associated with Trilinos, along with related policies, or pointers to the related policies.

### Public Developer Repositories

+ **Trilinos**:  The primary source code repository, commonly referred to as the "Trilinos repository", is a git repository that stores all Trilinos packages that have been through the copyright process. The repository can be checked out by developers from the [GitHub repository for Trilinos](https://github.com/trilinos/Trilinos). This repository serves as the basis not only for release tarballs, but also for the public. Therefore, **any changes pushed to the Trilinos repository will be released externally**. **Do not push anything to the Trilinos repository that cannot or should not be released externally**. If you push something accidentally that should not have been pushed, contact the Trilinos Framework Team immediately.

+ **Inserted Packages**: Several Trilinos packages were split off from the main Trilinos git repo when it was moved to GitHub.  These repositories are listed in the [Trilinos/cmake/ExtraRepositoriesList.cmake](https://github.com/trilinos/Trilinos/blob/master/cmake/ExtraRepositoriesList.cmake) file.  These repositories are cloned using the Trilinos/clone_extra_repos.py script (run using `cd Trilinos ; ./clone_extra_repos.py`).

### Protected Development Repositories

The following Trilinos-related repositories are located on the machine `software.sandia.gov` and are not publicly available.

+ **preCopyrightTrilinos**: The preCopyrightTrilinos git repository stores packages that have not yet been through the copyright process. Changes made to this repository will not be released externally initially, but will eventually be released after copyright is granted and the package is moved from preCopyrightTrilinos into the main Trilinos repository. This includes all commits in the history of the development of the package.

+ **TrilinosWeb**:  The TrilinosWeb CVS repository stores the source code for most non-Doxygen Trilinos website content, for both the Trilinos Developer site (from TrilinosWeb/version3), and Trilinos User site (from TrilinosWeb/version4). Changes to the TrilinosWeb repository are automatically uploaded to the website multiple times per day.

+ **TrilinosData**:  The TrilinosData CVS repository stores large data sets for multiple packages.
TrilinosSQE: The TrilinosSQE CVS repository contains a variety of documents and artifacts related to Trilinos Software Quality efforts.

+ **TrilinosDocuments**: The TrilinosDocuments SVN repository stores a variety of documents related to Trilinos as a whole and Trilinos packages.

+ **Trilinos3PL**: The Trilinos3PL CVS repository has historically stored copies of source code for some Trilinos third-party libraries, as well as compiled libraries for select platforms. This repository has no known active users.

+ **cvsConversion**:  The cvsConversion git repository contains scripts and utilities for moving packages into preCopyrightTrilinos and from preCopyrightTrilinos to Trilinos and scripts for creating and updating the public Trilinos repository. This repository is not of general interest to developers.

+ **TrilinosWebScripts**: The TrilinosWebScripts git repository contains the scripts necessary for updating the Trilinos User and Developer websites. This repository is not of general interest to developers.