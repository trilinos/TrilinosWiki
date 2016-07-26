Test writing is an essential software development auxiliary activities.  Without adequate, independent and easily executed tests, software viability is at risk of breaking due changes in upstream dependencies, build environments, internal refactorings and enhancements.  Under-tested software is unsustainable.  

### Trilinos Testing Policy:
1. _**All functionality will be covered by testing:**_ Numerous approaches are possible. Chapter 22 of Steve McConnell's book _Code Complete_ is an excellent resource. Structured Basis Testing (having as many tests as there are logic paths through your code) is a good starting point.  Formal unit testing is even better.
2. _**Untested functionality is itself a bug:**_ If any user or developer identifies an untested code segment in a _Production Growth_ package, there is a right and obligation to report it.
3. _**Contributed code must be covered:**_ While we appreciate non-developer contributions, they are subject to the same code coverage requirements.  In particular, Trilinos developers may refuse to accept contributions that do not have tests.

### Notes:
1. _**Policy Scope:**_ This policy formally applies to any _Production Growth_ package development.  It is encouraged for packages in other phases (_Research Stable_ or _Production Maintenance_).
2. _**Exceptions:**_ Yes, there will be exceptional circumstances where you need to commit some functionality without a corresponding test, but it should be the _exception_ and addressed as soon as possible.  Issues with management, user or client support for taking time to write tests should be forward to the Trilinos project leader.
3. _**Test writing is part of your day job:**_ Time spent writing tests is a core job responsibility, justifiable as a normal part of your work day.
4. _**Quick check for testing compliance:**_ While not sufficient, one simple indicator for testing compliance is whether or not a commit log message lists changes to both `src` and `test` directories.

### Questions?
Contact Mike Heroux <maherou@sandia.gov>

## Helpful Links:

[Code Complete, 2nd Edition](http://www.stevemcconnell.com/cc.htm), by Steve McConnell