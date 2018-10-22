# What is a *release*?

A *release* of [Interlibr](https://github.com/Xalgorithms/interlibr) is a fully
deployable, complete, revision of the [main
meta-project](https://github.com/Xalgorithms/interlibr). It may contain numerous
updates in each of the related projects, each with their own version.

# How is a *release* prepared?

A release is represented by a change in the version number of the the [main
meta-project](https://github.com/Xalgorithms/interlibr). Additionally, each of
the submodules associated with subprojects will be updated to reference the
relevant updates in the subproject.

## Development planning

For a new release of the Interlibr solution, *features* will be added as [Github
issues](https://github.com/Xalgorithms/interlibr/issues). These issues will be
labelled with the *planned release version* for the feature. Releases are
planning in tandem: the current release and the next release are planned
together so that incomplete features can be rolled into the next release. Once a
release is completed, a new *next release* will be generated.

*Features* in the Interlibr project are intended to be *cross-project*,
complete, improvements to the entire solution. The relevant changes *within* a
subproject are called *tasks*. In the [change protocol](./changes.md), we
provide more details.

The team uses
[waffle.io](https://waffle.io/Xalgorithms/xadf-active-repositories) to plan and
track work. The *departing* and *scheduled* columns in the boad track the
current and next releases. The *preflight* column tracks things that will be
implemented sometime in the future but not necessarily in the next release.

*Experimental*: When work starts on a release, a milestone should be added to
each of the issues in the column for the release. Any additional tasks that are
created while working on the releases should be added to the milestone.

## Deployment

When a release is fully tested and the team is confident in the changes, the new
solution will be deployed to our Google Cloud cluster. Before this occurs,
individual contributors can use a [Docker Compose
composition](https://github.com/Xalgorithms/interlibr/tree/master/ops/docker-compose)
to run a variant of the solution.

## Versioning

As with all projects maintained by the collective, [semantic
versioning](https://semver.org/) is used to version the [main
meta-project](https://github.com/Xalgorithms/interlibr).

## Change logs

When a new version of a project maintained by the collective is released, it is
required to [update a change log](https://keepachangelog.com/en/1.0.0/). This
hold true for the overall Interlibr project. That changelong will include *why*
changes were made to *each subproject*. It will also include a discussion of
*what has conceptually changed* for the project *as a whole* in the new
revision.

