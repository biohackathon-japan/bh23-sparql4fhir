---
title: 'BioHackJP 2023 Report R4: Enabling SPARQL queries over conventional medical records'
title_short: 'BioHackJP 2023 FHIR SPARQL'
tags:
  - SPARQL
  - FHIR
  - Linked Data
  - Data Integration
  - RDF
authors:
  - name: Eric Prud'hommeaux
    orcid: 0000-0003-1775-9921
    affiliation: 1
  - name: Claude Nanjo
    orcid: 0009-0002-1208-8858
    affiliation: 2
  - name: Jerven Bolleman
    orcid: 0000-0002-7449-1266
    affiliation: 3
affiliations:
  - name: Janeiro Digital
    index: 1
  - name: Medontology LLC
    index: 2
  - name: SIB Swiss Institute of Bioinformatics
    index: 3
date: 30 June 2023
cito-bibliography: paper.bib
event: BH23JP
biohackathon_name: "BioHackathon Japan 2023"
biohackathon_url:   "https://2023.biohackathon.org/"
biohackathon_location: "Kagawa, Japan, 2023"
group: R4
# URL to project git repo --- should contain the actual paper.md:
git_url: https://github.com/biohackathon-japan/bh23-sparql4fhir/tree/main/paper
# This is the short authors description that is used at the
# bottom of the generated paper (typically the first two authors):
authors_short: Eric Prud'hommeaux \emph{et al.}
---

# Background

HL7 FHIR is a global standard for accessing and sharing electronic medical records in secure environments. FHIR has its own custom query language called FHIR Path. FHIR Data has been available as RDF since release R2, and reached tech maturity level 3 but is not yet normative. FHIR RDF is supported by a number of FHIR platforms.

## Vision

Every single clinical system that has a FHIR compliant datastore will be able to allow access to this data using SPARQL. Leveraging their existing infrastructure and security systems, while giving researchers and medical staff the capability for analytical queries that are outside of the design sweetspot of FHIR Path. 

# Goals

While FHIR RDF is already a very practical standard to use. The most common way to query data in RDF sources is called SPARQL. Loading all data from an existing FHIR Source and loading it into a separate SPARQL capable database is not ideal. For reasons of security and data freshness.

We aimed to translate SPARQL to FHIR PATH syntax. This works only for a small subset of SPARQL as the query languages are quite different in their expressive power. Therefore we aimed to use an existing SPARQL engine and use its evaluation strategy to ensure correct SPARQL semantics. Doing this we will maintain all options for an as efficient as possible retrieval strategy from FHIR PATH capable datastores. Including filter pushdown and constant bindings.  

# Outcomes

We have a working prototype that allows for SPARQL queries matching the FHIR RDF R5 specification to be answered by a standard SPARQL protocol compliant endpoint. This includes general SPARQL optimisations but also pattern recognition. 

The engine chosen for this project is the Apache Jena’s ARQ engine. In this engine architecture we only need to replace standard parts of the SPARQL algebra with specific operators that we call HapiOps. HapiOps: use the hapi java clients for FHIR PATH over REST to talk to a FHIR data resource. HapiOps are the glue between the standard Jena SPARQL engine evaluation and the FHIR Path query language. Using the FHIR REST client as the base for our implementation means that this SPARQL engine can use any compliant FHIR data resource.

The code recognizing which SPARQL fragments can be mapped to a FHIR Path query was derived from the FHIR RDF ShEx data shapes. Originally evaluated in JS this has been translated into Java. The concept of the translation is based on ArcTrees. TODO: explain by Eric.

![Caption for BioHackrXiv logo figure](./biohackrxiv.png)

# Future work

We currently do not push down SPARQL filters into the where clauses of the FHIR Path. The code that manages the conversion of FHIR Path result bundles into SPARQL result sets are far from ideal in respect to long term performance. Significant testing to ensure that no FHIR concepts have been forgotten or mistranslated. 

SPARQL and OWL allow completing FHIR path result queries when using coding systems that have logical extensions such as Snomed CT and LOINC. E.g. querying for a more general LOINC code should retrieve all observations coded with a more specific child LOINC code. Efficient implementation of the consequences of OWL derived query rewriting into FHIR Path is an open issue that needs more development efforts.

## Acknowledgements

We would like to thank the fellow participants at BioHackathon 2023 for their collaboration and constructive advice, which greatly influenced our project. We are grateful to the organizers for providing this platform and the developers of open source language models. Special thanks to our mentors, advisors, and colleagues for their guidance and support. Without their contributions, our project in linked data standardization with LLMs in bioinformatics would not have been possible.

## References

1.
