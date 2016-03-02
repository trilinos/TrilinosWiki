Below is a concise summary of the key elements of processing new Trilinos issues in GitHub. This summary relates only to Trilinos-specific conventions. Other activities, such as making sure an issue is valid, and gathering enough information to address the issue, are omitted.

1. **Add Labels**: The Trilinos Project has a policy of requiring at least one label per GitHub issue. There are several types of labels to consider:

    * Package: Tpetra, Framework, etc.

    * Category: performance, test, build, bug, enhancement

    * Platform: linux, osx, windows, gpu, manycore

    (Note: we are currently seeking feedback on the correct types of and specific labels for the project)

2. **Mention Associated Teams and Developers**: Adding a GitHub mention for package teams and developers will alert the right people when a new issue is filed.

3. **Assign the Issue**: Assigning an issue insures that someone feels ownership of the issue. *Issues tied to important deliverables to be completed during the current and next calendar quarter should have an assignee.*

4. **Associate the Issue with a Milestone (optional)**: There are two types of milestones commonly used in the Trilinos Project.
    * Specific deliverable milestones. These milestones are created for specific deliverables with deadlines. Milestones of this type are sometimes tracked in a customer issue tracker. It is not always necessary to use a milestone if labels and other tracking mechanisms are sufficient. The names of these milestones should be descriptive (but short), but do not need to follow any formatting guidelines.

    * Quarterly milestones. These milestones help organize issues that need to resolved by a specific date that are not tied to specific deliverable milestones. Milestones of this type are of the format FY17Q2 (due by end of FY 2017, quarter 2).