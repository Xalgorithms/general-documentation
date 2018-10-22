# Context

Originally, much of the prototyping of Interlibr was done in Ruby using a number
of frameworks (Rails, Sinatra, etc). When development of the core platform
called Interlibr was started (Fall 2017), the SMACK conceptual stack was
selected as the basis of the platform. Public services were written in Ruby +
Sintra and internal services (Spark jobs) were written in Scala.

Over time, it became apparent that Scala would dominate the platform
development. Therefore, the team decided that the **majority** of the platform
would *eventually* be written in or moved to Scala. The only service that would
remain in Ruby would be the [revisions
service](https://github.com/Xalgorithms/service-il-revisions-github) due to a
reliance on the Rugged gem. During the effort, it was also decided that the
[events service](https://github.com/Xalgorithms/service-il-events) would remain
in NodeJS.

*This work was completed in 2018 August*
