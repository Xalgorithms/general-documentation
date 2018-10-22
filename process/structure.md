# Interlibr meta project

The *solution* implemented by the
[Interlibr](https://github.com/Xalgorithms/interlibr) project is a
*loosely-coupled* assemblage of projects. To various degrees, each of
the projects is, itself, a *useful* service that could be integrated
into different solutions. As each service is developed, *some care* is
taken to verifying that *minimal* Interlibr *conceptual dependancies*
are introduced. Throughout the services, this goal is met with some
success and some failure. Some of the projects are intrinsic to the
overall solution and some are less coupled.

To foster the goal of a loosely-coupled solution, each subproject is
maintained in a distinct repository with an independant version. The
[Interlibr](https://github.com/Xalgorithms/interlibr) meta-project is
the sum of these subprojects with additional configuration and
tooling that is *specifically* tailored for the solution.

# Organization of subprojects

As mentioned, each subproject is retained in a distinct repository
with independant versioning. To compose the overall Interlibr
solution, each subproject appears as a [git
submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules) in the
Interlibr repository. As the Interlibr solution is revisioned, it is
associated with particular revisions of each subproject.

The subprojects follow a loose organizational convention:

* *docs*: projects which contain no executable code, merely thoughts
  and notes about the solution
  
* *services*: independantly running *microservices* offered in Docker
  containers; any of these services may be run independant of the
  solution though their utility may vary
  
* *libs*: supporting libraries (typically Ruby gems or Scala JARs)
  that are useful to multiple services
  
# Adding or removing projects

