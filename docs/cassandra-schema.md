# Summary

Cassandra has a semi-structured schema system that we will use to
store *well-formed* JSON documents representing invoices and purchase
orders. The stored data will be structured using Cassandra's *user
type* mechanism.

For example, consider representing an invoice's envelope. We will
define user types for the parties in the envelope and the envelope
itself. That *top-level* type will be used in a table called
"invoices".

```
# create the code type
CREATE TYPE xadf.code (id text PRIMARY KEY, version text, list_id text, list_name text);

# create the person type
CREATE TYPE xadf.person (surname text, names set<text>);

# create the party type
CREATE TYPE xadf.party (id text PRIMARY KEY, address frozen <address>, industry frozen <code>, location frozen <location>, contact frozen <person>)

# create the envelope type
CREATE TYPE xadf.envelope (issued datetime, version text, buyer frozen <party>, customer frozen <party>);

# create the invoices table
CREATE TABLE xadf.invoices (id text PRIMARY KEY, envelope frozen <envelope>);
```

This will allow inserts using JSON structured to match the arrangement
of types.

```
INSERT INTO invoices JSON '{ "id" : "", "envelope" : { "issued" : "", "version" : "", "buyer" : { ... }, "customer" : { ... } } }`;
