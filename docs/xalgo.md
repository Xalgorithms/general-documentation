# Summary

This document explains the format of rules in XALGO 0.2.0. It includes
the textual representation of the rule as well as the prescribed
layout of rules as stored in Git repositories. Included with this
explanation is a high-level explanation of rule storage in the XADF.

# Components of Rule Packages

## Packaging

Rules operate on *JSON documents*. For the initial release of the
XADF, these documents are refined JSON representations of UBL
documents.

Rules are packaged and distributed using Git repositories. The
repository must follow a specific layout with specific naming
conventions. Each top-level directory in the repository represents a
single *rule package*. A single rule package is made up of three types
of files:

- *Package*: This is a single file that contains metainformation about
  the package including rule versions, effective dates and
  jurisdications. It **must be** named <ns>.package where <ns> is the
  name of the directory that constitutes the particular rule
  package. The format of this file is described in the next section.

- *Tables*: These are files that contain tabular data that is specific
  to the rule and that may be shared with other rule packages. Tables
  are stored in CSV format. The file names **must be** in the format
  <name>.table and correspond to an entry in the .package file.

- *Rules*: A series of map/reduce steps to be performed on a single
  document. Rules are stored in the *XALGO expression language*. The
  file names **must be** <name>.rule and correspond to an entry in the
  .package file.

## Package meta data format

The package file follows this format:

```
{
  "rules" : {
    // we assume this corresponds to a rule file in this directory
    // called 'foo.rule'
    "foo"    : {
      // a specifically generated identifier documented in the
      // entity id section
      "id" :      "RLcs303f",
      // version of this rule (http://semver.org)
      "version"   : "1.2.33",
      // ISO-8601 datetimes for when this rule should be applied
      // these require UTC offsets (see effective section)
      "effective" : [
        // timezone: tz style or abbrev
        { "timezone": "AST", "starts" : "2007-04-05T12:30", "ends" : "2008-04-05T12:30" }
      ],
      "jurisdiction" : {
        // ISO 3166-1 (alpha2, alpha3 or numeric)
        "country" : "CA",
        // ISO 3166-2 - must be valid in the country
        "region"  : "QC"
      },
      // specific XA requirements (see section on XA requirements)
      "xa" : {
        "version", "1.2.33"
      },
      // (see section on criticality)
      "criticality" : "high",
      // canonical URL where the source of the this rule/table is retained
      // if missing, generated from the repo URL
      "url" : "",
      // see section on roles
      "roles" {
        "manager" : {
          "name"  : "",
          "email" : "",
          "url"   : ""
        },
        "author" : {
          "name"  : "",
          "email" : "",
          "url"   : ""
        }
        // generated from repository metadata, reproduced here for
        // clarity based on the roles section
        "committer" : {
          "name"  : "",
          "email" : "",
          "url"   : ""
        }
      }
    }
  },
  "tables" : {
    "foo" : {
      // same
    }
  }
}
```

As illustrated in the example's comment lines, the file is a map of
component type and name to meta data that applies to the similarly
named file in the same direction. Everything specified in this file is
**mandatory** with the exception of the committer role that is
generated based on the latest commit referencing this file in the
repository.

### Rule Id

Rules and tables have a **very particular** identification scheme used
to provide a unique id for the component. This id *includes*
information about the component so that some minor decisions can be
made knowing only the id. The scheme is structured as follows:

- [0]: The first character in the id denotes the type of entity. It
  can be either S, R or T (signifying System, Rule or Table)
  
- [1]: The second character in the id MAY be populated with the ASCII
  character § to indicate that one or more subsequent characters is carrying
  semantic meaning from a globally-standardized ontology. When these positions
  re used they can contain any valid ASCII character except space available 
  on an ISO/IEC 9995 keyboard. The mandatory   sequential order of ontologies will be assigned 
  through an (as yet   underdeveloped) IoR governance process, from left to right. Use of two §§
  in a row signifies that the next designated ontology is not relevant. Use of the addition 
  sign with a number §+3§ indicates that the next three designated ontologies 
  are not relevant. Use of Ø indicates that the Rule ID character string will include no further use
  of the designated ontologies. But after the Ø any rule author or group of authors may 
  apply their own semantic coding structure or semantic text summaries. 

- [2..]: The first globally-standardized ontology is the International Standard 
  Industrial Classification [ISIC Rev.3] (https://unstats.un.org/unsd/cr/registry/regcst.asp?Cl=2)
  A letter (A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q) refers to a category for the component.
  If a rule is deemed relevant to more than one category, then two or more letters may be 
  indluded in the Rule ID string, before § or Ø appear.
  
- [3..]: These next locations in a Rule Id may be used for references to globally-
  standardized ontologies. 

- [22-46]: Any valid ASCII character except space available on an
  ISO/IEC 9995 keyboard. Optionally, this can be an abbreviated form
  of the rule name derived from the file name, unless the string is already in use.

*This identifier is optional*. If not present it will be added by the
authoring system described below. The six characters will be derived
from the name of the notebook, unless the resulting string is already in use.

### Rule Version

The rule version should follow [SemVer](http://semver.org). *This
value is optional*. If it is not present, it will be derived from the
release tag.

### Effective times

The effective times section of the component meta information
indicates a time period when the component is valid for use. Outside
of this period, the rule **will not** be applied to documents. These
times are expressed in **local time** with an attached time zone
(using a common abbreviation or the [IANA tz
database](https://www.iana.org/time-zones) specifier). Since a
juridiction may include *multiple* time zones, this field allows
specifying multiple effective dates.

### Jurisdication

The *jurisdiction* of a component is specified with a combination of
ISO-3166-1 and ISO-3166-2 codes. Both fields are optional. To make the
rule **apply globally**, the jurisdication key should be omitted from
the meta information.

### Criticality

A component can indicate criticality. Typically, this will control the
ordering of the rule against other rules of higher or lower
criticality. Valid values are

- *imperative*: Must always be active
- *business*: Must be active to uptime guaranteed tolerances
- *experimental*: May be deactivated to support business and
  imperative requirements

### Roles

The meta information specifies three roles:

- *manager*: This is a person responsible for the applied section of
  the rule. For example, they may be a public servant in the taxation
  department of a government.
  
- *author*: This is a person primarily responsible for the ongoing
  correctness of the *rule's implementation*.
  
- *committer*: The last person to make a change to this
  component. This is not specified in the meta information, it will be
  generated when the rule is updated.


## XALGO 0.2.0 Expressions

Rules are represented using the syntax of the XALGO 0.2.0 expression
language. This is so that there is an easily readable version of the
rule's logic retained somewhere in the system. It is this format that
should be committed to the rule package git repository. Other tools
may represent the rule in this format or they may present the rule
differently, depending on the representation needs of that tool.

```
# rule only applies to invoices
WHEN envelope:type == 'invoice';

# rule only applies to suppliers in the consumer petrol industry
WHEN envelope:parties.supplier.industry.code.value == 'G4711';

# rule only affects items that are petrol products (non-wholesale) as
# classified using the UNSPSC scheme
WHEN item:classification.code.list_name == 'UNSPSC';
WHEN item:classification.code.value == '506505';

# empty sales are not affected
WHEN item:quantity.value > 0;

# pseudo naming of tables (incomplete)
# REQUIRE ca-qc:tables:gas_taxation:distances;
# REQUIRE ca-qc:tables:gas_taxtions:reductions;

# build a table of reductions that are relevant to the supplier in the invoice
ASSEMBLE seller_reductions
  COLUMN distance FROM distances WHEN envelope:parties.supplier.id.value == distances.EffectiveUserID
  COLUMN reduction FROM reductions WHEN distances.distance <= reductions.2R3(a)_Distance;

#FORMULA calculate_reduction(a, b, c) multiply(a, subtract(b, c)))

# build a table derived from the items table in the invoice that
# includes a price reduction calculation
MAP items
 USING price_reduction = multiply(@quantity.value, subtract(@price.value, first(seller_reductions).reduction)));


# indicate a revision of the document
REVISE items
 USING @price.value = subtract(@price.value, @price_reduction);
```

Generally, the statements in an XALGO rule fall into specific categories:

- *refining the applicability of the rule*: The effectiveness and
  jurisdication of a rule are high-level constructs used to broadly
  reduce the rule search space. Using *WHEN* statements *inside* the
  specification of the rule, an author can more specifically tune the
  applicability. These can apply to the *envelope* of a document or
  specific predefined *parts* of the document (in this example, the
  items of an invoice).
  
- *assembling tables*: A rule is provided with predefined tables, but
  it may also need to build more computing tables based on the input
  document. These are built in memory during the computation phase of
  the rule and referenced by name.
  
- *map/reduce*: These are statements that expand or refine tabular
  data during the computation of the rule.

- *revision*: These are explicit statements about with computed data
  should be output into the new document revision.

# Storage

The full content of rules and tables are stored in the XADF document
database. Information related to rule matching and inference is stored
in Cassandra. Further details about this and the matching logic are
available in [data storage](data.storage.md).

# Authoring with Jupyter

## Structure

Rules and tables (as rule packages) are edited in [Jupyter
notebooks](http://jupyter.org/) alongside informational documentation
about the rule package. A single Jupyter notebook represents an entire
rule package. To support this relationship, we will add kernels and UI
to the core Jupyter project and host an instance of the system
alongside the XADF. This will be built on
(JupyterHub)[https://jupyterhub.readthedocs.io/en/latest/]. Notebooks
created in this instance will be persisted to GitHub repositories as
rule packages (described above).

When creating a notebook using the project's instance of JupyterHub,
the notebook will be required to contain a reference to a GitHub
repository. When the notebook is saved, it will be committed (or
updated) in the root of the repository. A directory with the same name
as the notebook will also be created. This directory will contain the
rule package that is associated with the notebook. Any rules or tables
that are created within the UI of the notebook will be persisted in
this directory and **merely referenced** from within the
notebook. Based on information captured using UI extensions, the
packaging data will also be updated in the rule package. This includes
versioning, selection of effective dates, selection of jurisdictions
and assignment of roles. The roles themselves will be associated with
users from the original JupyterHub instance.

## Authoring and Publishing

Any user of the XA Authoring environment based on JupyterHub will be
automatically allocated a *sandbox* for editing the notebook, tables
and rules. This environment will be preserved as a specially named
*branch* in GitHub. As the author makes changes to the package or its
contents, changes will be *comitted* to their sandbox branch. The
package on such a branch can be deployed to a *sandbox* within the
Fabric (see Execution) with the same name as the author's snadbox
branch. When they want to *publish* these changes to the *official*
version of the package, the branch will be *merged* into the *master*
branch of the repo and a new version tag (this is the version of the
**entire repository**) will be created. This publication will trigger
a *rebuild* of the package within the Fabric.

In addition to the master branch of the repo, there will exist a
*development* branch that an author can optionally select as a target
for their changes. This is a fully functional edition of the rule
package running *live* on the Fabric with precisely the same
capabilities as a *production* version published on the *master*
branch. The *development* branch has some differences from the
*master* (or *production*) branch:

- incoming documents must be *specifically targeted* to run using
  rules from the branch

- references to tables within a rule in the package will automatically
  use the *development* version of the referenced table even if there
  is a production version of the table with the same version

# Rules on the Fabric

## Deployment

As described above, Rule packages are deployed to the Fabric when a
merge occurs on the *master* branch of the repository. The deployment
steps are:

1. *Revisions Service* detects a merge and pulls the branch from
   GitHub

1. For all rules or tables in the package that have updated versions,
   new versions are added to the document database; any package-level
   data is updated
   
1. The job processing tables in Cassandra are updated

As soon as this deployment is completed, incoming documents will be
processed using the latest versions of the rules.

## Sandboxes

If a user would like to test their rules, the XADF will provide a
*sandbox environment* on the Fabric. This will allow the user to run
their rules against simple input documents. To isolate the changes
that the user is testing in their sandbox, GitHub branches will be
used. When the user is happy with their changes, they will be able to
merge the sandbox into the master branch of the GitHub repository **as
a new version**. As with versions, additional UI will be added to the
JupyterHub application.

# Syntax
