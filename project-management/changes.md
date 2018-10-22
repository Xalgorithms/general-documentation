# Intent

The purpose of our change protocols allow the entire collective to understand
*what* needs to be done, *who* improved the changes, *who* approved the changes
and *where* the changes have been integrated. We endeavour to minimize the
amount of process required to contribute a change, but some structure is
required to keep track of what we have done or what we intend to accomplish.

# Planning and structuring

The collective has usurped some common terminology for our own purposes. To
avoid confusion, this section specifically describes how we use the terms.

## Features

A *feature* is a set of behaviours that apply to the complete Interlibr
platform. It may encompass changes to several subprojects in order to complete
the entire feature. Features are created as Github issues in the Interlibr
project.

## Tasks

While a *feature* encompass many changes across many projects, a *task* is a
change to a single project that will be one part of a *feature*. When a
collaborator decides to work on a particular feature, they may be responsible
for creating the tasks for each project. In some cases, the collective may
already have created some tasks for a feature. It is expected that there is some
unknown when starting work on a feature, therefore contributors are not expected
to create *all* tasks when they start on a feature. They may add tasks as they
are discovered.

A task should be created as a Github issue within the specific subproject. In
the description, there should be a link to the associated feature.

# Defects

Eventually, a defect will be discovered in a project. Defects should be added as
Github issues associated with the relevant project.

## Planning board

The collective uses a [waffle planning
board](https://waffle.io/Xalgorithms/xadf-active-repositories) for task
management. A peculiar set of stages is used in the board:

*Preflight*: These are *features* that the team plans to do someday. When
scheduling a new release, features from preflight may be added to the release.

*Scheduled*: These are *features* and *tasks* scheduled for the *next*
release. The version number for the next release appears in the column header.

*Departing*: These are *features* and *tasks* scheduled for the *current*
release. The version number for the next release appears in the column header.

*In Flight*: These are *features* and *tasks* that some contributor is actively
working on. They came from the *Departing* column. Contributors are encouraged
to return features and tasks to the *Departing* column when not actively
working.

*Landed*: These are These are *features* and *tasks* that are complete and will
be in the current release when it is completed.

# Branching and Pull Requests

The collective uses a branching model that is loosely inspired by
[OneFlow](https://www.endoflineblog.com/oneflow-a-git-branching-model-and-workflow#variation-develop-master). Contributors
are encouraged to read and understand that model.

When working on a feature, defect, etc, (or at least to submit a PR),
contributors should create a branch named `<type>/<##>/<name>` where `<type>` is
the sort of task (`features`, `defects`, usually), `<##>` is the Github issue
related to the work and `<name>` is a contributor-specific string that allows
them to remember what is in the branch.

The collective prescribes a minimal, formal commit format:

```Simple, short description of the change

<issue reference>

Longer, multiple paragraph description of the change. We expect to understand
WHY this change was made and WHAT this change is in this longer description.
```

In this format, `<issue reference>` can simply be a link to the issue or it can
be a special *active keyword* used by
[Github](https://help.github.com/articles/closing-issues-using-keywords/) or
[Waffle](https://help.waffle.io/faq/waffle-workflow/use-waffles-connect-keyword-to-connect-prs-to-issues). Examples:
`Closes #nn`, `Connects to #nn`.
