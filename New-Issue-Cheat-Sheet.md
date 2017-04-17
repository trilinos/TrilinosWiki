Below is a concise summary of the key elements of processing new Trilinos issues in GitHub. This summary relates only to Trilinos-specific conventions. Other activities, such as making sure an issue is valid, and gathering enough information to address the issue, are omitted.

The full Trilinos Testing Policy can be found at: [https://github.com/trilinos/Trilinos/wiki/Managing-Trilinos-Project-Issues](https://github.com/trilinos/Trilinos/wiki/Managing-Trilinos-Project-Issues).

0. **Close the Issue if it will never be worked**: If the package lead or the capability lead for the area (for an orphaned package) reads over an Issue and decides that it is not appropriate, or realistically will never be resolved, then it is appropriate to close the Issue and state the justification for why the Issue should be closed.  Other interested parties should be @ referenced in the comment that closes the issue. Closing issues rather than leaving them open for years prevents false expectations on the part of Trilinos customers. Closed issues can be found and reopened if the issue priority increases.

1. **Add Labels**: The Trilinos Project has a policy of requiring at least one label per GitHub issue. There are several types of labels to consider:

    * Package: `Tpetra`, `Framework` (includes CMake, TriBITS, configuration), etc.

    * Category: `performance`, `test`, `build`, `bug`, `enhancement`

    * Platform: `linux`, `osx`, `windows`, `gpu`, `manycore`

    (Also see the "help wanted" in the "Assign the Issue" item below.)

2. **Mention Associated Teams and Developers**: Adding a GitHub mention for package teams and developers will alert the right people when a new issue is filed.

3. **Assign the Issue**: Assigning an issue insures that someone feels ownership of the issue. If no developer appears to have current or future capacity to address the issue, the `help wanted` label should be applied to the issue. *Issues tied to important deliverables to be completed during the current and next calendar quarter should have an assignee listed (or the `help wanted` label applied) to clearly identify staffing issues.* See also "Close the Issue if it will never be worked" above.

4. **Associate the Issue with a Milestone (optional)**: There are two types of milestones commonly used in the Trilinos Project.
    * Specific deliverable milestones. These milestones are created for specific deliverables with deadlines. Milestones of this type are sometimes tracked in a customer issue tracker. It is not always necessary to use a milestone if labels and other tracking mechanisms are sufficient. The names of these milestones should be descriptive (but short), but do not need to follow any formatting guidelines.

    * Quarterly milestones. These milestones help organize issues that need to resolved by a specific date that are not tied to specific deliverable milestones. Milestones of this type are of the format FY17Q2 (due by end of FY 2017, quarter 2).