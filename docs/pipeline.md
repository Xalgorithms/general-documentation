# Summary

Document processing in XADF occurs along a *pipeline* of jobs loosely
connected via Kafka topics. Each Job in the pipeline performs a
specific function (similar to a (cloud
function)[https://en.wikipedia.org/wiki/Function_as_a_service]) and
submits the results of that function to a subsequent Kafka topic. The
majority of these are implemented as Spark Jobs (therefore we commonly
call all of them *jobs*), but there is no hard requirement that they
must be Spark Jobs. Some of the processors may be implemented in a
different way that could be more suitable to the required work. The
primary Jobs are:

1. Assembling the rules which are valid in a document's jurisdication and efficacy (effective rules)
1. Filtering these rules based on envelope criteria (applicable rules)
1. Loading the tables associated with the rule
1. Running individual rules against the document's table of items

# Effective rules

A rule is *effective* if it matches the jurisdiction (country and
region) specified by a document. The jurisdication will be drawn from
the participating parties in the envelope of the ingested document
(refer to (documents)[./documents.md] for this structure). A party
(one of "supplier", "customer", "payee", "buyer", "seller", "tax" or
"any") must be elected for the *effectiveness* of the rule. These
correspond to the (parties in
UBL)[http://docs.oasis-open.org/ubl/csprd01-UBL-2.2/mod/summary/reports/UBL-Invoice-2.2.html]. The
*any* selector indicates that the rule is effective if **any of the
parties** fall within the rule's jurisdication.

If effective dates and times are specified in the (rule
package)[./xalgo.md], then the issued date and time in the document
**must fall within** this effective period.

# Applicable rules

A rule is *applicable* if **all** of the *when conditions* pertaining
to the *envelope* section specified in the rule are met.

# Loading required tables

A rule may specify a number of data tables to be loaded into the
execution context of the rule. This will result in *dynamic* tables
being created in Cassandra. These tables will be *named* according to
the name and version of the table specified. The data in the tables
will be loaded from the CSV tables that are stored in ArangoDB when
the rule is published.

The reason that we have decided to *actually import* the table data
into Cassandra (rather than merely load it into memory) is
twofold. First, we feel that the data needs to be truly in-memory
during the rule execution for performance reasons. It is a complicated
process to determine perform this dynamic loading, therefore we have
decided that a distinct Spark Job should be used for this task. Since
there is a distinct job, we require some place to retain this data
between phases. The simplest place to keep it is in Cassandra.

# Execution of rules

## Execution context
