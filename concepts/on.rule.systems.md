# On the Nature of Rule Systems

The functionality of the Interlibr platform is often - in this documentation -
referred to as a *rule system*. This can be a confusing terms in some contexts
because there are a number of different interpretations of the term "rule
system". In this document, we outline the major categories of *rule systems* and
compare the Interlibr and Xalgo implementations with those categories.

## Terminology

Often we will use specific terms that **may not** met common understanding of
those terms. In this section, we define those terms:

* *document*: We will often use *document* to refer to *passive*
  information. That is purely representative data with no active behaviours
  associated. This may refer to objects in a data model, tables in a database or
  other data structures that are submitted to *rules systems* for processing.

* *event*: This used to represent something *active* with occurred within a
  software platform. This may be user activity, internal platform behaviours,
  etc.

## Categories of Rule Systems

### Event / Condition / Action (ECA)

This category of *rule system* often appears in event-processing platforms (IoT
platforms, for example). The *rules* in this category are usually simple
declarative statements that include: an event type, a set of conditions and an
action to be taken. These rules express the idea that *when* an event arises, if
certain conditions are met (based on information accompanying the event), then a
specific action should be taken.

Pure implementations of this class of rule system are *stateless* with no
accumulation of information between invocations of a rule. This effective
prohibts this category from performing *inference* based on the activation of a
particular rule; there being no information preserved from which to draw
inference.

An example rule might be:

```
WHEN light_switch_is_toggled IF switch_is_up THEN signal_light(false)
WHEN light_switch_is_toggled IF switch_is_down THEN signal_light(true)
```

In this example, for a theoretical IoT light control system, one event arrives
into the system: `light_switch_is_toggled`. The switch might be up or
down. Depending on the position, the `signal_light` action is triggered with the
correct new state for the light.

### Inference / Classification

This is a common pattern in *expert systems* and it is often referred to as a
*production rule system*. A *body of knowledge* is built up using a database of
predicates that partially categorize information submitted to the system. Often,
systems of this category follow the
[RETE](https://en.wikipedia.org/wiki/Rete_algorithm) pattern-matching algorithm
to build a network of related inferences.

There are two common variants of this category of rule system: pure inference
and action-by-inference (ABI). Pure inference systems merely classify or answer
inqueries about the information sumbitted. ABI implementations add an additional
processing step that may trigger an action based on the categorization of the
sumbmitted information.

Action-by-inference systems are superficially similar to ECA systems but can be
differentiated if we examine the semantics surrounding the information that is
submitted to the rule system in order to trigger an *evaluation* of that
information. ECA systems act on *something that occurred* (a *verb*,
essentially) and attempt to answer the question: *Since this event occurred,
what should we do now?* ABI systems receive *documents* (some information - a
*noun*) and classify it in order to answer the question: *What is this and what
should we do with it?*

A simple example of inference rules would be:

```
parent("Bob", "Bill")
parent("Mary", "Bill")
parent("John", "Mary")

ancestor(X, Y) = parent(X, Y)
ancestor(X, Y) = parent(X, P) AND ancestor(P, Y)

ancestor?("John", "Bill")
```

This example sets up a basic set of *atomic facts*: *Mary* and *Bob* are parents
of *Bill*, *John* is a parent of *Mary*. Someone (`X`) is an ancestor of someone
else (`Y`) if they are a parent of that person **or** if they are a parent of
some third person (`P`) who is also an ancestor of `Y`. This allows us to ask a
question: Is *John* an ancestor of *Bill*?

This is the most common type of rule system. It has been popularized in the
semantic web domain by standards like [Rule Interchange Format
(RIF)](https://en.wikipedia.org/wiki/Rule_Interchange_Format) and the broader
community associated with [RuleML](https://en.wikipedia.org/wiki/RuleML). It has
long been in use with deductive databases and associated language like
[Datalog](https://en.wikipedia.org/wiki/Datalog). Many business rule
([BRMS](https://en.wikipedia.org/wiki/Business_rule_management_system)) systems
like [Drools](https://en.wikipedia.org/wiki/Drools) include inference rule
systems with declarative languages (often mixed with some workflow elements).

In many of these exemplar implementations, the inference engine and rules
submitted to it are often coupled with elements of the other categories in this
document.

### Workflow

Often, a rule system must process *documents* according to a prescribed
procedure or recipe. Some of these steps may require user intervention and
others may occur automatically. Rule systems of this category encode the steps -
the *path* through the procedure - that must occur using a (typically)
declarative *workflow language*.

The purpose of this category of system is to factor the domain *out* of the
processing platform. This places responsibility for the correct construction of
complex domain-specific rules in the hands of the (presumed) more capable domain
experts rather than platform developers. The critical identifying feature of a
workflow rules system is that it **does not** make inferences. Rather, any
formal language associated with the system encodes **how** a procedure will be
carried out (essentially, a recipe).

Workflow systems are many and varied. They often appear as an informal feature
of a larger software platform or are integrated directly into other
software. For most developers, they will encounter workflow rules systems as
part of a configuration management system (Kubernetes configuration; Ansible,
Chef deployment scripts, etc) or a package management system (Debian or RPM
software packaging, for example). Product owners may encounter the category as
part of business process management system ([Apache
ODE](https://en.wikipedia.org/wiki/Apache_ODE) is one example using
[WS-BPEL](https://en.wikipedia.org/wiki/Business_Process_Execution_Language) to
specify the rules).

### Transformative

In many cases, information in a solution may need to be *transformed* from one
representation (or context) to another. For example, from XML to JSON. Rules can
be written to generalize the solution. In this category, rules in the solution
are usually written in declarative language that indicates (using building
blocks provided by the rule system) the mechanics of transforming on context to
the other.

A rule system that fits this category is a very inflexible, mechanical system
usually dedicated to internal processing pipelines rather than more user-facing
components. This is because the purpose of this sort of system is to remove
tedious and repetitive code from the platform itself, replacing it with
generalized operations that can be specifically instantiated for particular
data.

# Categorizing Interlibr and Xalgo

To understand how (or if) Interlibr and Xalgo fit into this categorization a
brief review of what the platform does is required. If we examine each of the
operational steps in the process, we can understand how the platform relates to
these categories. The simplified [execution
pipeline](https://github.com/Xalgorithms/general-documentation/blob/master/pipeline.md)
for Interlibr is:

1. accept *hierarchical documents* expressed as JSON
1. match *rules* to those documents according to two specific categories
   (*effective* and *applicable*)
1. construct an execution context based on submitted documents
1. execute a series of map/reduce/filter operations expressed in *Xalgo rules*
   on those tables with supplementary information from supporting tables
   expressed as *Xalgo tables*
1. emit *revisions* to the original document as a key/value set

## Classification of Effective and Applicable

The operations performed in this stage of the pipeline bear a superficial
resemblance to the operations performed by an *inference rule system*. There are
several criteria that determine each of these classifications. All of these
criteria *could be* written as predicates in Datalog or Drools. On closer
examination, it is easy to see that we would only ever ask two questions:
`effective?` or `applicable?`. Since we have only these questions, then the set
of *facts* that we need to register within the rule system will be *very
finite*.

There are two ways to establish these facts:

* *effective*: The effectiveness of a rule is determined by matching key/value
  pairs submitted with the document against a corresponding set of values keyed
  against rule ids (and stored in Cassandra).

* *applicable*: Applicability is merely the system determining whether or not a
  rule will generate revisions of a submitted document. The applicability of a
  rule is expressed as part of the rule itself *in Xalgo*. This was a design
  choice to make rule implementation easier for rule authors (by allowing them
  to see these criteria in the same file as the rule that are writing).

From these descriptions, we can easily see that since *effectiveness* is
determine by a simple relational algebra expression between two key/value sets,
it does not match any of the categories above. Certainly, it does not match the
qualities of an *inference* system.

Applicability, on the other hand, *does* match the qualities of an *inference*
system. Within
[Xalgo](https://github.com/Xalgorithms/general-documentation/blob/master/xalgo.md),
applicability is expressed as a series of boolean expessions based on key and
values from the *execution context* (as `WHEN` statements). This could be very
effectively represented in a system like Datalog or Drools. The **reason this
has not been done** is that we wanted to retain a symmetry of syntax with Xalgo
(for reasons mentioned above). At the time the language was designed, it was
simpler to implement a *boolean expression evaluator* rather than implement or
(more likely) integrate a complex inference engine that offered **far more
capabilities** that was required. We developed a fit-for-purpose method of
representing the boolean expressions in Cassandra tables, allowing evaluation of
the expressions using a relational method similar to that used for
effectiveness.

_Conclusion_: There is a boolean evaluator in one part of this stage of the
pipeline that loosely matches an *inference* system. Using an _actual_ inference
engine for this component would not introduce a value that is proportional to
the difficultly of implementing or integrating one.

## Constructing an initial context

Since the Xalgo interpreter performs map/reduce/filter operations using document
and table structures, we need a [mechanism to extract those
structures](https://github.com/Xalgorithms/interlibr/issues/46). This is a
simple key/value map submitted with the document. Internally, when a rule is
evaluated, the Interlibr rule interpreter determines the *type* (document or
table) of the referenced elements in the context. If the expected type of the
evaluation point matches the type of the provided element, then evaluation
continues. Otherwise, evaluation fails.

_Conclusion_: The syntax (a JSON configuration) submitted with the document to
be processed represents a _very rudiamentary_ form of a *transformative* rule
system. It is *merely* a one-to-one mapping, so its' usefulness outside of this
very narrow context is non-existent.

## Executing the rules

## Emitting revisions

Interlibr emits revisions as a series (officially: an event log) of key/value
pairs. The key in a revision *represents* a reference into the originally
submitted document. Submitters can use these revisions to modify the document
*however they see fit*. Interlibr imposes no structure in this case, it merely
*informs*.

_Conclusion_: This bears no resemblance to any category of rules system.
