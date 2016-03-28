# Policies for and List of Trilinos Repositories

## Introduction

When we refer to the "Trilinos repository", we are referring to the primary Trilinos git repository located at software.sandia.gov:/space/git/Trilinos. However, there are a large number of repositories associated with the Trilinos Project. Below is a list of repositories associated with Trilinos, along with related policies, or pointers to the related policies.

### General Interest Repositories

+ **publicTrilinos**: The Trilinos public repository is a read-only git repository based on a recent version of the primary Trilinos git repository. The policies associated with this repository are maintained here, and information about accessing the repository can be found here.

### Developer Repositories

+ **Trilinos**:  The primary source code repository, commonly referred to as the "Trilinos repository", is a git repository that stores all Trilinos packages that have been through the copyright process. The repository can be checked out by developers with accounts on software.sandia.gov. The location of the repository is software.sandia.gov:/space/git/Trilinos. This repository serves as the basis not only for release tarballs, but also for the public repository. Therefore, **any changes pushed to the Trilinos repository will be released externally**, barring significant efforts to prevent release. **Do not push anything to the Trilinos repository that cannot or should not be released externally**. If you push something accidentally that should not have been pushed, contact the Trilinos Framework Team immediately. The public repository is updated frequently, but not continuously based on changes to the Trilinos repository

+ **preCopyrightTrilinos**: The preCopyrightTrilinos git repository stores packages that have not yet been through the copyright process. Changes made to this repository will not be released externally initially, but will eventually be released after copyright is granted and the package is moved from preCopyrightTrilinos into the main Trilinos repository. This includes all commits in the history of the development of the package.

+ **TrilinosWeb**:  The TrilinosWeb CVS repository stores the source code for most non-Doxygen Trilinos website content, for both the Trilinos Developer site (from TrilinosWeb/version3), and Trilinos User site (from TrilinosWeb/version4). Changes to the TrilinosWeb repository are automatically uploaded to the website multiple times per day.

+ **TrilinosData**:  The TrilinosData CVS repository stores large data sets for multiple packages.
TrilinosSQE: The TrilinosSQE CVS repository contains a variety of documents and artifacts related to Trilinos Software Quality efforts.

+ **TrilinosDocuments**: The TrilinosDocuments SVN repository stores a variety of documents related to Trilinos as a whole and Trilinos packages.

+ **public_Trilinos**:  The public_Trilinos git repository is a staging area for the publicTrilinos repository. public_Trilinos updates automatically from Trilinos, but the update from public_Trilinos to publicTrilinos is manual at this time. This repository is not of general interest to developers.

+ **Trilinos3PL**: The Trilinos3PL CVS repository has historically stored copies of source code for some Trilinos third-party libraries, as well as compiled libraries for select platforms. This repository has no known active users.

+ **cvsConversion**:  The cvsConversion git repository contains scripts and utilities for moving packages into preCopyrightTrilinos and from preCopyrightTrilinos to Trilinos and scripts for creating and updating the public Trilinos repository. This repository is not of general interest to developers.

+ **TrilinosWebScripts**: The TrilinosWebScripts git repository contains the scripts necessary for updating the Trilinos User and Developer websites. This repository is not of general interest to developers.