# IS and OUGHT Tables: Introducing the "is-ot-bot"

Any device with Internet connectivity is able to provide or obtain two types of simple documents in the form of ordered lists (tuples) expressed in plain text through JavaScript Object Notation (JSON). 

* An “IS” document (isdoc) has column headings with a single row of fields populated with letters or numbers to express some factual circumstances at a particular date and within particular jurisdictions. 

* An “OUGHT” document (otdoc) has column headings with one or more rows of  fields populated with letters or numbers to express hypothetical circumstances, but also each row ends in an assertion column to express a rule using any functional characters and syntax supported in Standard Query Language (SQL). 

The IoR is ‘live’ when the otdoc repository is coupled with a data processing robot (referred to as the is-ot-bot). Whenever an isdoc arrives, the is-ot-bot runs a map/reduce/filter operation against all available otdocs. It first selects all otdocs with date and jurisdictional ranges encompassing the date and jurisdication field values of the arriving isdoc. Those are the rules "in-effect" for that isdoc. Then the is-ot-bot uses the rest of the isdoc’s field values to filter the in-effect otdocs for any other matching fields. Those that remain are the rules which are "in-effect" and “applicable” for that isdoc. Then the is-ot-bot uses the assertion column from the otdoc to run a process using data from that isdoc and the selected in-effect+applicable otdocs. If any of the otdoc assertions invoke other otdocs the is-ot-bot obtains and runs them. With the temporary set of applicable otdocs held in memory, the assertions may be run in no particular order, or in a specified sequence. 
