# Summary

As documented in detail elsewhere, the projects managed by the
Foundation fall into four high-level categories:

- **Lichen**: A UBL document processing system that sends business documents to
  the Interlibr for rule discovery and execution. This categories includes web
  and mobile applications that interact with Lichen document services.
  
- **XAlgo**: A high-level declarative language for specifying rules that operate
  on hierarchical documents (JSON-like).

- **Interlibr**: A compute service that performs rule discovery and execution
  for using the XAlgo specification.
  
- **XalgoAuthor**: An authoring interface for tables and rules that will be
  executed by Interlibr.
  
# Active Projects

## Integrated Solutions

### [Interlibr](https://github.com/Xalgorithms/interlibr)

Contains the Docker and Kubernetes deployment configurations and includes the
Ruby/Thor based Interlibr CLI.

## Common Libraries

### [Xalgo Rule Parser](https://github.com/Xalgorithms/lib-rules-parse-ruby)

This Ruby gem uses Parslet to offer a parser for the Xalgo syntax. The output of
the parser is a JSON representation of the Xalgo.

### UBL Parser

A simplistic UBL parser written in
[Ruby](https://github.com/Xalgorithms/lib-ubl-ruby) and
[Javascript](https://github.com/Xalgorithms/lib-ubl-js).

### [Xalgo Rule Interpreter](https://github.com/Xalgorithms/lib-rules-int-scala)

This Scala library is a reference implementation of the Xalgo Execution
Model. The team refers to this implementation as the *naive interpreter*.

### [Platform Storage](https://github.com/Xalgorithms/lib-storage)

A storage abstraction layer in Scala used by several of the Scala services.

## Services

A service is an independantly running process typically following the
micro-service architecture pattern. In Interlibr, services are typically
deployed as Docker containers on Kubernetes.

### [Spark Jobs](https://github.com/Xalgorithms/services-il-jobs)

All of the Spark processing performed in Interlibr is written in Scala and
compiled to a single JAR.

### [Revisions/Github](https://github.com/Xalgorithms/service-il-revisions-github)

This Ruby/Sinatra service receives action-oriented requests related to rules and
tables that are stored in Github repositories. It can receive notifications from
Github to update stored rules and tables. Most of its processing is scheduled as
Sidekiq jobs.

### [Schedule](https://github.com/Xalgorithms/service-il-schedule)

This Scala service written with Play and Akka handles all inbound processing
requests.

### [Query](https://github.com/Xalgorithms/service-il-query)

This Scala service written with Play and Akka handles all requests for data
stored in the platform.

### [Execute](https://github.com/Xalgorithms/service-il-execute)

This Scala service written with Akka process all rule executions requests which
are delivered via Kafka queues.

### [Events](https://github.com/Xalgorithms/service-il-events)

This NodeJS service offers a web socket subscription service that can be using
by integrated services to receive notifications of processing events.
