Software testing is one of the essential auxiliary activities when developing software.  Without adequate, independent and easily executed testing, software viability is at risk of breaking due changes in upstream dependencies, build environments, internal refactorings and enhancements.  Under-tested software is unsustainable.  

### Trilinos Testing Policy:
1. _All functionality will be covered by testing:_ Numerous approaches are possible. Chapter 22 of Steve McConnell's book _Code Complete_ is an excellent resource. Structured Basis Testing (having as many tests as there are logic paths through your code) is a good starting point.  Formal unit testing is even better.
2. _Untested functionality is itself a bug:_ If any user or developer identifies an untested code segment in a _Production Growth_ package, there is a right and obligation to report it.
3. _Contributed code must be covered:_ While we appreciate non-developer contributions, they are subject to the same code coverage requirements.

### Notes:
1. _Exceptions:_ Yes, there will be exceptional circumstances where you need to commit some functionality without a corresponding test, but it should be the _exception_ and addressed as soon as possible.  Issues with management, user or client support for taking time to write tests should be forward to the Trilinos project leader.
2. _Test coding is a part of your day job:_ Time spent writing tests is a core job responsibility, justifiable as a normal part of your work day.
3. _Quick check for testing compliance:_ While not a sufficient, one simple indicator for testing compliance is whether or not a commit log message lists changes to both src and test directories.

### Questions?
Contact Mike Heroux <maherou@sandia.gov>

## Helpful Links:

[Code Complete, 2nd Edition](http://www.stevemcconnell.com/cc.htm), by Steve McConnell