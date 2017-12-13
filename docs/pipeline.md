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
1. Running individual rules against the document's table of items

# Effective rules

# Applicable rules

# Execution of rules
