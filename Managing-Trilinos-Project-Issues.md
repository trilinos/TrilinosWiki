(Draft: seeking feedback on overall content, especially labels and milestones)

[Overview](#overview)

[Using GitHub Issues](#using-github-issues)

[Using Waffle.io (Kanban Board)](#using-waffle)

[Processing New GitHub Issues](#processing-new-issues)

## <a name="overview"></a>Overview

The Trilinos Project uses the Kanban agile software development process to improve project productivity and increase visibility and transparency of activities. The primary issue tracking tool used by the Trilinos Project is [GitHub Issues](https://guides.github.com/features/issues/). In addition, [waffle.io](https://waffle.io) provides a [Kanban Board](https://waffle.io/trilinos/Trilinos) view of [Trilinos Issues on GitHub](https://github.com/trilinos/Trilinos/issues). Waffle.io uses the same login credentials as GitHub. 

In addition to GitHub Issues and waffle.io, some Trilinos deliverable tracking takes place in the issue tracking tools of Trilinos customers. This tracking can be any combinations of high-level, (similar to SCRUM epics), or at a lower-level (similar to SCRUM stories or tasks) depending on specific circumstances. The process for using customer tools varies by customer. 

The basic premise of Kanban is:

1. By limiting the number of distinct project tasks that are active (in progress) at any given time, productivity is improved because the overhead of swapping between distinct activities is optimized. 

1. Weaknesses in the development workflow that create bottlenecks in productivity become evident when an in-progress task is taking longer than expected. Rather than permitting the team to move on to another task, the team must identify and fix the productivity bottleneck, leading to a better process in the future.

1. Avoids a deadline-based approach. Deadlines are dealt with in a different way.

1. Provides a nice board for viewing and managing issues. The board is useful for all stakeholders, including developers, interested users, project leadership, and management.

An excellent overview of Kanban (and Scrum) is provided by [Henrik Kniberg and Mattias Skarin](http://www.infoq.com/minibooks/kanban-scrum-minibook).

## <a name="using-github-issues"></a>General Guidance for Trilinos Developers Using GitHub Issues 

GitHub provides an [excellent overview](https://guides.github.com/features/issues/) for using GitHub Issues. Developers should be generally familiar with the features discussed in this overview. Below we assume familiarity with these features and focus on recommended and common use of these features within the Trilinos project.

The Trilinos Project strongly encourages the use of labels, and _requires that each issue have at least one label applied to it_. Users who do not have write access to the repository cannot apply labels to new or existing issues. The justification for this GitHub (not Trilinos Project) policy is that developers would not want users applying the wrong labels to issues (consider for example if there was an "urgent" label, or something of that nature). While users can "mention" developers or package teams when filing issues, we cannot uniformly expect them to do so. Given this, there is a significant risk that an issue could be filed, and that issue could be overlooked for an extended period of time. To mitigate this risk, we require that every issue have at least one label applied to it. The process for executing this policy, and further details are provided below in the next section.

Many Trilinos package development teams have created a [team in the Trilinos organization on GitHub](https://github.com/orgs/trilinos/teams). By doing this, teams can be "mentioned" in the same way that users can be mentioned. The cost of having a team is simply the maintenance of appropriate team membership. Currently, it is not required that each package have a team associated with it in the Trilinos organization. This is at least partially due to the fact that some packages are orphaned and when maintenance or development needs to be performed on those packages, it can be best to have a specific individual tasked with the effort, rather than notifying multiple people, none of whom may take ownership of the issue.

GitHub Issues does not support dependency tracking in a strong way. That is to say, you cannot say that one issue depends on another in the same way you can in a tool like Bugzilla. There are two ways dependencies can be noted using GitHub issues. First, you can use a reference, as noted in the overview. References can denote a dependency, or simply that another issue is "related" (a designation that is lacking in Bugzilla). If you reference one issue in another, the referenced issue will have a note added to it saying it was referenced, and listing the issue it was referenced by. The other way a dependency can be noted is to set up a milestone and add the issue to that milestone. For example, if customer release 1.0 depends on a GitHub issue, you can set up a milestone for customer release 1.0. You can also track progress toward milestones.

## <a name="using-waffle"></a>General Guidance for Trilinos Developers Using waffle.io

As stated above, waffle.io provides a [Kanban Board](https://waffle.io/trilinos/Trilinos) for [Trilinos GitHub Issues](https://github.com/trilinos/Trilinos/issues), and a basic premise of Kanban is to limit work-in-progress. "Work-in-progress" relates specifically to the tickets in the third column of the Kanban board "In Progress". Below is a brief description of each of the columns on the Kanban board.

* **Backlog**: The Backlog column is where all GitHub Issues start out. The Backlog is just what it says - a backlog of work. These tickets have not necessarily been identified as valid, and are not necessarily detailed enough to being work on.

* **Ready**: Once a ticket has been verified as valid, and there is enough detail to start working on the issue, it can be moved from Backlog to Ready. Optionally, some packages may want to impose the additional requirement that the package lead has chosen the issue to be worked on in the near future. In this way, the package leader can identify the next tasks to be completed, and the rest of the team can choose from the issues selected for development. In this case, the package leader is the only one who moves work from the Backlog to Ready column. In reality, issues move very quickly from the Backlog to In Progress, and may essentially skip this column, for example when a defect is uncovered in the code that needs to be addressed quickly. For larger development tasks that need to be clearly defined prior to starting work, the concept of "Ready" is critical.

* **In Progress**: When work has begun on an issue, it should be moved from Ready to In Progress. Before an issue is moved to In Progress, someone should be assigned to the issue. If multiple people are working on the issue, it is acceptable to select one to be the assignee, or to break the issue into multiple issues, each with a different assignee. Note that the assignee can be changed as different parts of the issue are worked on. The idea is that there is a clear contact for the work at any time. NOTE: While Kanban strives to limit work-in-progress, the Trilinos Project has not imposed formal work-in-progress limits for packages or developers. Prior to beginning work on another issue, developers are asked to look quickly at their current In Progress tickets, and those of colleagues who might need help moving forward on a blocked issue. Often it is better to finish a ticket already in progress first to avoid excessive task swapping. As progress is made on an In Progress issue, developers are asked to add comments to the issue to keep stakeholders informed.

* **Done**: When work on an issue is completed (including any review), it can be moved to Done. This is most commonly accomplished by simply closing the issue in GitHub Issues, or though a git commit message. This represents no additional overhead to a typical developer's workflow. Developers are discouraged from simply dragging an issue from In Progress to Done without making some kind of comment.

Note: On the waffle.io Kanban board for Trilinos, it is possible to simply drag and drop issues from one column to another. If you are working in GitHub, and prefer not to look at the waffle.io Kanban board at that moment, it is possible to move issues from column to column by applying labels also. For example, if you want to move from "Backlog" to "Ready", add the label "ready" to the issue. 

### Filtering the Kanban Board

While the default waffle.io Kanban board for Trilinos provides some insight into the amount and type of work going on in the project, looking at all of the issues can quickly become overwhelming. Issues can be filtered by title or label using the "Filter Board" box in the upper right-hand corner of the screen. The drop-down next to this box allows filters to be applied based on label, assignee, milestone, as well as a couple other more advanced features. Note that filters are "and'ed", not "or'ed". In other words, if you select "Kokkos" and "Tpetra", you will get the issues with both the Kokkos and Tpetra labels, not all issues with at least one of those two labels.

Filtered searches can be bookmarked. For example, the URL to the Kanban board for MueLu issues is [https://waffle.io/trilinos/Trilinos?label=MueLu](https://waffle.io/trilinos/Trilinos?label=MueLu).

## <a name="processing-new-issues"></a>Processing New GitHub Issues

(Note: a summary of this information is available on the [New Issue Cheat Sheet](https://github.com/trilinos/Trilinos/wiki/New-Issue-Cheat-Sheet))

Using the filtering capability mentioned above, it is possible to find all of the issues with no labels applied to them using the "Unlabeled" label filter. When issues are submitted without labels, the below steps should be taken to properly review and categorize the new issue. This can be done by any developer. Developers are asked when visiting the Kanban board to check periodically for issues without labels are carry out these steps. Developers are also asked to complete these steps when filing a new issue so that it is immediately properly categorized:

1. **Add Labels**: As previously mentioned, the Trilinos Project has a policy of requiring at least one label per GitHub issue. The idea is that only those who have push access to the repository can apply labels, so if labels have been applied to the issue, someone has at least done a preliminary review of the issue. Once a label is applied, it will no longer show up in the "Unlabeled" view, so it is important to also complete the remaining steps below, as other developers will not know these have not been completed once a label is applied. Applying all applicable labels is also important, because filtering by label will only work to the extent that the right labels have been added. Here are some common types of labels to consider:

    * Package: Tpetra, Framework, etc.

    * Category: performance, testing, build, bug, enhancement

    * Platform: linux, osx, windows, gpu, manycore

    (Note: we are currently seeking feedback on the correct types of and specific labels for the project)

2. **Mention Associated Teams and Developers**: Adding a GitHub mention for package teams and developers will alert the right people when a new issue is filed. It may also be appropriate to add comments to the issue when mentioning teams/developers.

3. **Assign the Issue**: If there is an obvious assignee for an issue, it is best to assign the issue so that someone feels ownership of the issue. If that person cannot address the issue due to time or other constraints, they can try to identify an alternative assignee. *Issues tied to important deliverables to be completed during the current and next calendar quarter should have an assignee listed to help identify staffing issues.* In the case where an issue is filed associated with a package that is "orphaned", i.e. no current developers are closely associated with package, it is particularly important to choose an assignee. Although assigning someone to an issue for a package that no one closely associates themselves with may be counter-intuitive, it is important to note that if that is the case, no one is likely to filter for the package label either, so pick someone, and they can try to find someone else if necessary.

4. **Associate the Issue with a Milestone (optional)**: There are three types of milestones commonly used in the Trilinos Project.
    * Specific deliverable milestones. These milestones are created for specific deliverables with deadlines. Milestones of this type are sometimes tracked in a customer issue tracker. It is not always necessary to use a milestone if labels and other tracking mechanisms are sufficient. The names of these milestones should be descriptive (but short), but do not need to follow any formatting guidelines.
    * Quarterly milestones. These milestones help organize issues that need to resolved by a specific date that are not tied to specific deliverable milestones. Adding issues to these milestones, and then filtering by label or assignee helps developers to see what items need to be completed within a specific timeframe. Milestones of this type are of the format FY17Q2 (due by end of FY 2017, quarter 2).