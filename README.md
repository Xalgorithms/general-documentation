README: XALGORITHMS REGISTRY & LICHEN COMPONENT

    *** TABLE OF CONTENTS ***

    WHAT IS THIS PROJECT?
    INTELLECTUAL RIGHTS MANAGEMENT & LICENSING
    HOW TO CONTRIBUTE
    PROJECT MANAGEMENT
    SOURCE CODE MANAGEMENT & DISTRIBUTION
    CODING STANDARDS
    HOW TO BUILD
    HOW TO INSTALL
    TROUBLESHOOTING
    FIRST AUTHORS

WHAT IS THIS PROJECT?

Participants in Xalgorithms Alliance have created a specification, a platform-agnostic component, and a compute method so that any computational algorithms can be readily transmitted from any independent source repositories within which they are maintained, to any applications that would use them. 

Thus we envision a scalable enhancement of "The Internet" to enable, in effect, an Internet of Rules. [Main Website](https://www.xalgorithms.org/)

**An Internet of Rules is designed to solve the general class of problem in which agent A, interacting with agent B, would obtain one or more externally-managed computational rules from agents C..n, where:
* A and B may or may not know about C..n's rules, or about any updates to them, but either or both would prefer to be notified about relevant rules when interacting.
* C...n may or may not know about A and B in particular, nor about their particular medium of interaction, but can expect A or B or their medium of interaction to be capable of exchanging data with a generic medium common to A..n.
* A and B would tolerate the risk of exposing limited data through the generic medium in order to learn of applicable rules from C..n.

This is being created as a distributable, no-fee, table-structured reference catalogue of computational algorithms of any type. Each rule package in on the data fabric includes its authoritative uniform resource identifier, a table-oriented expression of the rule, its ongoing version history, and its terms of use. The registry contains just the information required for each individual rule to be invoked. Xalgorithms (“ex-algorithms”) refers to external extensible algorithms. 

The Lichen class of components will be for installation adjacent to or semi-embedded into any application with a suitable API that requires automated access to external rules. Lichen is named after the diverse and colorful family of symbiotic living things that attach to the surfaces of other objects, without interfering in their internal functions or integrity. See: [How a Buyer or Seller Would Use Lichen:](https://github.com/Xalgorithms/xa-arch/wiki/How-a-Buyer-or-Seller-Would-Use-Lichen)

This project serves diverse use cases, however our reference implementation applies to commerce. Xalgorithms prototyping will support network-based automation of taxes, exemptions, duties, credits, loyalty incentives, algorithmic pricing, micro-payment and multi-payment structuring, agent-based scrutiny of transaction patterns according to user-defined constraints, and disclosure-controlled multi-scale data generation for real-time or trend analytics.

INTELLECTUAL RIGHTS & LICENSING

Xalgorithms Alliance aligns with both the demand-side perspective of The Free Software Definition (i.e. user freedoms), and the supply-side perspective of The Open Source Software Definition (i.e. developer methodologies).

The intellectual rights section of the [Xalgorithms Alliance Accession Agreement](https://www.xalgorithms.org/about-us/membership/) (XAAA) is adapted from the [Fedora Project Contributor Agreement, Version 2015-02-03](https://fedoraproject.org/wiki/Legal:Fedora_Project_Contributor_Agreement?rd=Legal:FPCA)  Our common objective is to ensure that contributions have the desired free/libre/open licensing terms without requiring intellectual rights assignment. 

In summary, when a committer submits an unlicensed contribution to this project, Xalgorithms Alliance relies on the following default licenses:
* [Apache 2.0 or a more recent version](https://www.apache.org/licenses/LICENSE-2.0)
* [GNU GPL 3.0 or a more recent version](https://www.gnu.org/licenses/gpl-3.0.en.html)
* [LGPL 3.0 or a more recent version](https://www.gnu.org/licenses/lgpl.html)
* [CC-by 4.0 or a more recent version](https://creativecommons.org/licenses/by/4.0/)

The first three licenses in this list include explicit non-aggression clauses relating to statutory monopolies over ideas (i.e. "patents"). Occassionally Xalgorithms Foundation may post [defensive publications](http://www.defensivepublications.org/) to pre-empt the imposition of such monopolies.

On an ad hoc basis, other free/libre/open source licenses may additionally be used by Xalgorithms Foundation in combination with the default licenses, to accommodate certain participation requirements other organizations, or to meet particular business objectives of the Alliance which are not fully accommodated in the above licenses.

In order to make the Lichen component readily adaptable to any deployment environment, it is being provided with two distribution streams from the outset:
* OpenLichen will comprise the core elements of the system, to be distributed under the permissive Apache License (AL) version 2.0 license, or a more recent version of it.
* FreeLichen will be a feature-rich derivative of OpenLichen, to be distributed under the unified GNU General Public License (GPL) version 3.0, or a more recent version of it.

HOW TO CONTRIBUTE (SOURCE CODE, SYSTEMS MANAGEMENT)

For collaboration the Xalgorithms team regularly uses:
* Github for code and task management: https://github.com/Xalgorithms. In the future components may also be mirrored on [Bitbucket](https://bitbucket.org/)... but we've not done this yet.
* Jupyter (via https://datascientistworkbench.com/) to document and test each algorithm's executable code and results 
* Markdown (Jupyter variant) http://datascience.ibm.com/blog/markdown-for-jupyter-notebooks-cheatsheet/  https://tools.ietf.org/html/rfc7764 with Pandas Dataframes https://pandas.pydata.org/pandas-docs/stable/dsintro.html
* Apache Spark, Mesos, Assak, Cassandra, Kafka, OpenWhisk https://gist.github.com/Arnauld/b98f41b7a245c859be34f6efa9627513 on AWS
* Universal business language (UBL) INVOICE and ORDER http://docs.oasis-open.org/ubl/os-UBL-2.1/UBL-2.1.html#T-INVOICE  http://docs.oasis-open.org/ubl/os-UBL-2.1/UBL-2.1.html#T-ORDER
* Tradeshift for UBL-conformant invoice testing https://tradeshift.com/roles/sb/small-business-free-invoicing/
* Slack for ad hoc communications: https://xalgo-lichen.slack.com/messages/ 
* Etherpad for ad hoc co-drafting: e.g. https://etherpad.wikimedia.org
* GoogleDocs for structured co-drafting: (default rendering in OpenDocument, e.g. ODT, ODP)
* GoogleHangouts for calls
* Xalgorithms.org for use cases: https://xalgorithms.org/#root=extended-capabilities
* Twitter @xalgorithms (140+ followers); @xalgo4trade (360+ followers)

Participation in technical work of Xalgorithms Alliance is by free/libre/open source licensing (in order to state terms and conditions for the distribution and use of works, respecting intellectual rights title) and by contract (in order to clearly state intellectual rights title). The contract may either be [explicit](https://www.xalgorithms.org/about-us/membership/) through the "XALGORITHMS ALLIANCE ACCESSION AGREEMENT" or [implied in fact](https://www.law.cornell.edu/wex/contract_implied_in_fact)  Participating under an implied contract shall always be assumed as carrying the same terms as the XALGORITHMS ALLIANCE ACCESSION AGREEMENT.

HOW TO CONTRIBUTE (BUSINESS MANAGEMENT, GOVERNANCE)

Participation in business management and governance of Xalgorithms Alliance is structured through an explict contract with Xalgorithms Foundation, namely the [XALGORITHMS ALLIANCE ACCESSION AGREEMENT](https://www.xalgorithms.org/about-us/membership/) 
Xalgorithms Foundation is a Canadian-based member-funded not-for-profit, incorporated in Canada. The sole purpose of the Foundation is to provide management and governance services to the Xalgorithms Alliance. And Xalgorithms Alliance exists as an unincorporated association of members committed to fostering a free/libre/open Internet of Rules to advance the fairness and efficiency of commerce.  

HOW TO CONTRIBUTE (FINANCIALLY)

Xalgorithms Foundation offers [five categories of membership in Xalgorithms Alliance](https://xalgorithms.org/participate/)

PROJECT MANAGEMENT

Business aspects of Xalgorithms are managed on [an instance of OpenProject](https://worksite.xalgorithms.org)

[Request an Account](https://worksite.xalgorithms.org/account/register)

CODING STANDARDS

[Under revision...]

Representational State Transfer (REST) Architectural Style 
http://www.restapitutorial.com/
https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm

ADAPTIVE Communication Environment (ACE) is a free/libre/open-source object-oriented framework for concurrent systems: http://www.cs.wustl.edu/~schmidt/ACE-overview.html

Software Package Data Exchange (SPDX) is a standard format for communicating the components, licenses and copyrights associated with a software package:
https://spdx.org/about-spdx

HOW TO BUILD

[Under development...]

HOW TO INSTALL

[Under development...]

TROUBLESHOOTING

[Under development...]

ORIGINS 

The initial business concept and general systems design of the Xalgorithms Registry and the Lichen Component were jointly conceived and authored by Joseph Potvin and William Olders in 2015, based upon their independent ideas developed throughout the previous 25 years. 

Potvin and Olders first met on 21 November 2014. Joseph Potvin's firm, The Opman Company, and William Olders' firm, DataKinetics, signed a contract on 24 December, 2014 to commence formal work on their shared concepts. Overall concept development through 2015 brought in specific ideas from Alain Zander, Randy McCoy and Larry Strickland, exectutives of DataKinetics. In later 2015, implementation details were contributed by free/libre/open systems developers Don Kelly, Patrick Naubert, and Michael Richardson. Also in later 2015, international trade economist David Hamilton contributed to further understanding the data significance of wide deployment. Throughout 2015, specific insights were gained through Potvin's participation in community deliberations of the W3C "Community Group on Web Payments", in the US Federal Reserve's "Faster Payments Task Force", and also though his role as Chair of the "Management Education Working Group" of the Open Source Initiative.

Potvin and Olders [incorporated Xalgorithms Foundation Inc. on 4 December 2015](https://www.ic.gc.ca/app/scr/cc/CorporationsCanada/fdrlCrpDtls.html?corpId=9537775&V_TOKEN=1458033564814&crpNm=xalgorithms&crpNmbr=&bsNmbr=)

Xalgorithms Foundation was provide start-up financial backing from DataKinetics, in the form of a 5-year [Tier 3 Membership](Membership structure is describe in detail at: https://www.xalgorithms.org/about-us/membership/)

The current technical team is led by Don Kelly and Bill Olders, also with requirements specification led by Joseph Potvin. Developers Hayk Pilosyan (full time) and Sr. Systems Developer Tigran Sargsyanand (part time) are creating the production-class system. The early proof-of-concept implementation in 2015 and 2016 was mainly done by Don Kelly with input from Patrick Naubert, Simon Deziel, Michael Richardson and Samir Hussain. 

