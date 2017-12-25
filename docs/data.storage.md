# Summary

This document explains the internal data storage of rules and
tables. It also explains the mechanisms for matching rules against
input documents. This is related to the (processing
pipeline)[pipline.md].

The XADF will use a heterogeneous data storage layer with multiple
data storage services that offer optimal storage and query
capabilities for particular contexts. For *raw storage* of documents,
revisions, rules and tables, (ArangoDB)[https://www.arangodb.com/]
will be used. This storage service is optimized for storing large
document-oriented data with simple querying requirements. This is
acceptable for our documents, revisions, rules and tables because we
are simply interested in *storing* the data and making simple
queries. For the Spark-based map/reduce processing, we will store
query-oriented tables in Cassandra. These tables will be used to
derive the data required by the Spark Jobs.

# Cassandra structure

The structure of tables in Cassandra will be determine by the types of
queries that will be needed to satisfy the Spark Jobs in the data
processing pipeline. Currently, these Jobs are:

1. Determining which rules are valid and effective in the
   jurisdication of a document. These are called the *effective*
   rules.
   
1. Filtering the effective rules based on envelope data in the
   document. These are called *applicable* rules.
   
## Effective rules

When new or updated rules are stored in the ArangoDB, this will cause
rows to be added or updated in the *effective* table in
Cassandra. This table has these columns:

* *rule_id*: A reference to the the rule as stored in ArangoDB
* *country*: An ISO-3166-1 country code (*null* if the rule applies in **all** jurisdications)
* *region*:  An ISO-3166-1 region code (*null* if the rule applies to **all** regions in the country)
* *timezone*: An IANA tz identifier
* *starts*: A date and time in *local time* indicating when the rule takes effect
* *ends*: A date and time in *local time* indicating when the rule ceases to take effect
* *party*: One of "supplier", "customer", "payee", "buyer", "seller", "tax" or "any"

## Applicable rules

To determine applicable rules, we use two tables in Cassandra. The use
of these tables is described in [processing pipeline](pipline.md).

We have one table which records **all** sections and keys that have
been used in submitted rules (in *WHEN* statements) to specify
applicability. The data in this table is used to prefilter envelope
data from an ingested document. It has these columns:

* *section*: The name of the document section that pertains to this
  condition.

* *key*: A dotted-path key that references a key in a section
   (eg. parties.seller.name, currency, period.starts...)
  
A second table will be used to record all conditions that a rule
should match:

* *rule_id*: A reference to the the rule as stored in ArangoDB

* *section*: The name of the document section that pertains to this
  condition.
  
* *key*: A dotted-path key that references a key in a section
   (eg. parties.seller.name, currency, period.starts...)
  
* *operator*: An envelope filter operator (see syntax for *WHEN* in
  [XALGO-2.0](xalgo-2.0.md].

* *value*: The required value for the *key* given the *operator*
  (stored as a string)
