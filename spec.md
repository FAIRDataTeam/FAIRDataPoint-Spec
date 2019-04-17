# FAIR Data Point metadata specification

The FAIR Data Point's metadata layered approach is composed of five layers, namely, the FAIR Data Point itself, the collection of datasets, each one of the offered datasets, the datasets' distribution and the structure and semantics of each of dataset.

For an overview of the FDP motivation and design, see [README.md](README.md).

Table of contents
===
- [FAIR Data Point metadata layer](#fair-data-point-metadata-layer)
- [Catalog, Dataset and Distribution metadata layers](#catalog-dataset-and-distribution-metadata-layers)
  - [Catalog metadata layer](#catalog-metadata-layer)
  - [Dataset metadata layer](#dataset-metadata-layer)
  - [Distribution metadata layer](#distribution-metadata-layer)
  - [Data record metadata layer](#data-record-metadata-layer)
- [Access rights rdf model](#access-rights-rdf-model)

## FAIR Data Point metadata layer

The FAIR Data Point metadata contains information about the FDP itself and its governing authority. The FAIR Data Point metadata content is based on the [RE3Data Schema](http://www.re3data.org/schema).

FDP metadata content table 

Ontology     | Term name                | Required/Optional | Description
------------ | ------------------------ | ----------------- | -----------
DC terms     | dct:title                | `Required`        | Name of the repository with the language tag
|            | dct:hasVersion           | `Required`        | Version of the repository
|            | dct:description          | Optional          | Description of the repository with  the language tag
|            | dct:publisher            | `Required`        | Organisation(s) responsible for the repository
|            | dct:language             | Optional          | 
|            | dct:license              | Optional          | 
|            | dct:conformsTo           | Optional          | The specification of the repository metadata schema (for example ShEx)
|            | dct:rights               | Optional          | 
|            | dct:references           | Optional          | Reference to documentation (API or otherwise).
|            | dct:accessRights         | Optional          | Description of the access rights, see [Access rights rdf model](#access-rights-rdf-model)
FDP ontology | fdp:metadataIdentifier   | `Required`        | Identifier of the metadata entry. Define new sub property ‘metadataID’ for dct:identifier 
|            | fdp:metadataIssued       | `Required`        | Created date of the metadata entry
|            | fdp:metadataModified     | `Required`        | Last modified date of the metadata entry
RDF Schema   | rdfs:label               | Optional          | Name of the repository with the language tag 
RE3Data      | r3d:institution          | Optional          | 
|            | r3d:startDate            | Optional          | Release date of the repository
|            | r3d:lastUpdate           | Optional          | Last update timestamp of the repository
|            | r3d:dataCatalog          | `Required`        | List of catalog metadata URLs
|            | r3d:country              | Optional          |  
|            | r3d:repositoryIdentifier | `Required`        | Identifier of the repository.

An example of FDP metadata

```ttl
    @prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
    @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
    @prefix dcat: <http://www.w3.org/ns/dcat#> .
    @prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
    @prefix owl: <http://www.w3.org/2002/07/owl#> .
    @prefix dct: <http://purl.org/dc/terms/> .
    @prefix lang: <http://id.loc.gov/vocabulary/iso639-1/> .
    @prefix fdp: <http://rdf.biosemantics.org/ontologies/fdp-o#> .
    @prefix r3d: <http://www.re3data.org/schema/3-0#> .
    @prefix foaf: <http://xmlns.com/foaf/> .
    
    <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp> dct:alternative "DTL FDP"@en ;
        dct:description "The DTL FAIR Data Point hosts the FAIR Data versions of datasets that have been made FAIR during BYODs as well as other relevant life sciences datasets"@en ;
        dct:title "DTL FAIR Data Point"@en ;
        dct:hasVersion  "1.0" ;
        dct:publisher   <http://dtls.nl> ;
        fdp:metadataIdentifier <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp-metadataID> ;
        fdp:metadataIssued "2016-10-27"^^xsd:date ;
        fdp:metadataModified "2016-10-27"^^xsd:date ;
    r3d:repositoryIdentifier <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp-repositoryID> ;
    r3d:dataCatalog <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/biobank> , <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics> , <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/patient-registry> , <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/textmining> , <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/transcriptomics> ;
    r3d:institution <http://dtls.nl> ;
    r3d:institutionCountry <http://lexvo.org/id/iso3166/NL> ;
    r3d:lastUpdate "2016-10-27"^^xsd:date ;
    r3d:startDate "2016-10-27"^^xsd:date ;
    a r3d:Repository ;
    rdfs:label "DTL FAIR Data Point"@en .
         
    <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp-metadataID> a <http://edamontology.org/data_0842>;
        dct:identifier "fdp-metadataID".
    
    <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp-repositoryID> a <http://edamontology.org/data_0842>;
        dct:identifier "fdp-repositoryID".
 
    <http://dtls.nl> a foaf:Organization;
        foaf:name "DTLS"@en.
```

## Catalog, Dataset and Distribution metadata layers

For the representation of the catalog of datasets, each one of the offered datasets and their distributions, we adopt as basis the [W3C's Data Catalog Vocabulary (DCAT)](http://www.w3.org/TR/vocab-dcat/). DCAT defines three main classes:
* dcat:Catalog: defines the catalog, i.e., the collection of datasets;
* dcat:Dataset: represents an individual dataset in the collection;
* dcat:Distribution: represent an accessible form of a dataset, e.g., a downloadable file or a web service that gives access to the data.

### Catalog metadata layer

Catalog metadata content table 

Ontology     | Term name              | Required/Optional | Description
--------     | ---------              | ----------------- | -----------
DC terms     | dct:title              | `Required`        | Name of the catalog with the language tag
|            | dct:hasVersion         | `Required`        | Version of the catalog
|            | dct:publisher          | `Required`        | Organisation(s)  or Persons(s) responsible for the catalog
|            | dct:description        | Optional          | Description of the catalog with the language tag
|            | dct:language           | Optional          | 
|            | dct:license            | Optional          | 
|            | dct:issued             | Optional          | Created date of the catalog entry
|            | dct:modified           | Optional          | Last modified date of the catalog entry
|            | dct:conformsTo         | Optional          | The specification of the catalog metadata schema (ShEx)
|            | dct:rights             | Optional          | 
|            | dct:accessRights       | Optional          | Description of the access rights, see [Access rights rdf model](#access-rights-rdf-model)
|            | dct:isPartOf           | `Required`        | Relation to the parent metadata.
FDP ontology | fdp:metadataIdentifier | `Required`        | Identifier of the metadata entry. Define new sub property ‘metadataID’ for dct:identifier 
|            | fdp:metadataIssued     | `Required`        | Created date of the metadata entry
|            | fdp:metadataModified   | `Required`        | Last modified date of the metadata entry
RDF Schema   | rdfs:label             | Optional          | Name of the catalog with the language tag
FOAF         | foaf:homepage          | Optional          | 
DCAT         | dcat:dataset           | `Required`        | List of dataset URLs
|            | dcat:themeTaxonomy     | `Required`        | List of taxonomy URLs
 
An example of catalog metadata.

```ttl
    @prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
    @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
    @prefix dcat: <http://www.w3.org/ns/dcat#> .
    @prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
    @prefix owl: <http://www.w3.org/2002/07/owl#> .
    @prefix dct: <http://purl.org/dc/terms/> .
    @prefix lang: <http://id.loc.gov/vocabulary/iso639-1/> .
    @prefix foaf: <http://xmlns.com/foaf/> .
    @prefix fdp: <http://rdf.biosemantics.org/ontologies/fdp-o#> .
    
    <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics> dct:hasVersion "1.0" ;
        dct:publisher   <http://dtls.nl> ;
        fdp:metadataIdentifier <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics-metadataID> ;
        fdp:metadataIssued "2016-10-27"^^xsd:date ;
        fdp:metadataModified "2016-10-27"^^xsd:date ;
        dct:issued "2016-10-27"^^xsd:date ;
        dct:modified "2016-10-27"^^xsd:date ;
        dct:language lang:en ;
        dct:title "Catalog for comparative genomics datasets"@en ;
        a dcat:Catalog ;
        rdfs:label "Catalog for comparative genomics datasets"@en ;
        dcat:dataset <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics/goNlSvR5> ;
        dcat:themeTaxonomy <http://dbpedia.org/resource/Comparative_genomics> , <http://edamontology.org/topic_0797> .
     
    <http://dtls.nl> a foaf:Organization;
        foaf:name "DTLS"@en.
    
    <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics-metadataID> a <http://edamontology.org/data_0842>;
        dct:identifier "comparativeGenomics-metadataID".
```

### Dataset metadata layer

Ontology     | Term name              | Required/Optional | Description
---          | ---                    | ---               | ---
DC terms     | dct:title              | `Required`        | Name of the dataset with the language tag 
|            | dct:publisher          | `Required`        | Organisation(s)  or Persons(s) responsible for the dataset
|            | dct:hasVersion         | `Required`        | Version of the dataset 
|            | dct:description        | Optional          | Description of the dataset with the language tag
|            | dct:conformsTo         | Optional          | The specification of the dataset metadata schema (ShEx)
|            | dct:issued             | Optional          | Created date of the dataset entry
|            | dct:modified           | Optional          | Last modified date of the dataset entry 
|            | dct:language           | Optional          | 
|            | dct:license            | Optional          | 
|            | dct:rights             | Optional          | 
|            | dct:accessRights       | Optional          | Description of the access rights, see [Access rights rdf model](#access-rights-rdf-model)
|            | dct:isPartOf           | `Required`        | Relation to the parent metadata.
FDP ontology | fdp:metadataIdentifier | `Required`        | Identifier of the metadata entry. Define new sub property ‘metadataID’ for dct:identifier 
|            | fdp:metadataIssued     | `Required`        | Created date of the metadata entry
|            | fdp:metadataModified   | `Required`        | Last modified date of the metadata entry
RDF Schema   | rdfs:label             | Optional          | Name of the dataset with the language tag
DCAT         | dcat:distribution      | `Required`        | List of distribution URLs
|            | dcat:theme             | `Required`        | List of concepts that describe the dataset
|            | dcat:contactPoint      | Optional          | 
|            | dcat:keyword           | Optional          | Keyword(s) related to the dataset with the language tag 
|            | dcat:landingPage       | Optional          | Home page of the dataset

An example of dataset metadata.

```ttl
    @prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
    @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
    @prefix dcat: <http://www.w3.org/ns/dcat#> .
    @prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
    @prefix owl: <http://www.w3.org/2002/07/owl#> .
    @prefix dct: <http://purl.org/dc/terms/> .
    @prefix lang: <http://id.loc.gov/vocabulary/iso639-1/> .
    @prefix foaf: <http://xmlns.com/foaf/> .
    @prefix fdp: <http://rdf.biosemantics.org/ontologies/fdp-o#> .
     
    <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics/goNlSvR5> dct:publisher    <http://www.nlgenome.nl> ;
        dct:description "The dataset contain 27.8k SV calls (>20bp). Calling was realized using 10 different approaches (see below) and a consensus strategy was used to produce this set. The SOURCE field in the INFO column lists all methods that called each of the events. As most methods do not report genotypes but rather presence/absence of an SV in an individual, we report here either a homozygous reference (0/0) in case of the absence of SV or a genotype with one alternative allele and one unknown allele (./1) in case of the presence of a SV"@en ;
        dct:hasVersion "1.0" ;
        fdp:metadataIdentifier <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics/goNlSvR5-metadataID> ;
        fdp:metadataIssued "2016-10-27"^^xsd:date ;
        fdp:metadataModified "2016-10-27"^^xsd:date ;
        dct:issued "2016-10-27"^^xsd:date ;
        dct:language lang:en ;
        dct:modified "2016-10-27"^^xsd:date ;
        dct:publisher <http://orcid.org/0000-0002-1215-167X> , <http://orcid.org/0000-0002-6816-4445> ;
        dct:title "GoNL human variants"@en ;
        a dcat:Dataset ;
        rdfs:label "GoNL human variants"@en ;
        dcat:distribution <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics/goNlSvR5/html> , <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics/goNlSvR5/textfile-gzip> ;
        dcat:keyword "GoNL" , "goNlSvR5" , "human" , "variant" ;
        dcat:landingPage <http://www.genoomvannederland.nl/> ;
        dcat:theme <http://dbpedia.org/resource/Homo_sapiens> , <http://dbpedia.org/resource/Mutation> .
     
    <http://www.nlgenome.nl> a foaf:Organization;
        foaf:name "The Genome of the Netherlands"@en.
     
    <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics/goNlSvR5-metadataID> a <http://edamontology.org/data_0842>;
        dct:identifier "goNlSvR5-metadataID".
```
### Distribution metadata layer

Ontology     | Term name              | Required/Optional | Description
--------     | ---------              | ----------------- | -----------
DC terms     | dct:title              | `Required`        | Name of the data distribution with the language tag
|            | dct:conformsTo         | Optional          | The specification of the distribution metadata schema (ShEx)
|            | dct:license            | `Required`        | Link to the license description
|            | dct:hasVersion         | `Required`        | Version of the distribution
|            | dct:issued             | Optional          | Created date of the distribution entry
|            | dct:modified           | Optional          | Last modified date of the distribution entry
|            | dct:rights             | Optional          | 
|            | dct:description        | Optional          | Description of the description with the language tag
|            | dct:accessRights       | Optional          | Description of the access rights, see [Access rights rdf model](#access-rights-rdf-model)
|            | dct:isPartOf           | `Required`        | Relation to the parent metadata.
FDP ontology | fdp:metadataIdentifier | `Required`        | Identifier of the metadata entry. Define new sub property ‘metadataID’ for dct:identifier 
|            | fdp:metadataIssued     | `Required`        | Created date of the metadata entry
|            | fdp:metadataModified   | `Required`        | Last modified date of the metadata entry
RDF Schema   | rdfs:label             | Optional          | Name of the data distribution with the language tag
DCAT         | dcat:accessURL         | `Required` (or dcat:downloadURL) | A landing page, feed, SPARQL endpoint or other type of resource that gives access to the distribution of the dataset
|            | dcat:downloadURL       | `Required` (or dcat:accessURL)   | A file that contains the distribution of the dataset in a given format
|            | dcat:mediaType         | `Required`        | (Only for dcat:downloadURL) 
|            | dcat:format            | Optional          | 
|            | dcat:byteSize          | Optional          | 
 


An example of distribution metadata.
```ttl
    @prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
    @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
    @prefix dcat: <http://www.w3.org/ns/dcat#> .
    @prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
    @prefix owl: <http://www.w3.org/2002/07/owl#> .
    @prefix dct: <http://purl.org/dc/terms/> .
    @prefix lang: <http://id.loc.gov/vocabulary/iso639-1/> .
    @prefix foaf: <http://xmlns.com/foaf/> .
    @prefix fdp: <http://rdf.biosemantics.org/ontologies/fdp-o#> .
     
    <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics/goNlSvR5/html> dct:hasVersion "1.0" ;
        fdp:metadataIdentifier <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics/goNlSvR5/html-metadataID> ;
        fdp:metadataIssued "2016-10-27"^^xsd:date ;
        fdp:metadataModified "2016-10-27"^^xsd:date ;
        dct:issued "2016-05-27T10:16:21+00:00"^^xsd:dateTime ;
        dct:license <http://rdflicense.appspot.com/rdflicense/cc-by-nc-nd3.0> ;
        dct:modified "2016-05-27T10:16:21+00:00"^^xsd:dateTime ;
        dct:title "GoNL web app"@en ;
        a dcat:Distribution ;
        rdfs:label "GoNL web app"@en ;
        dcat:accessURL <http://www.nlgenome.nl/search/> ;
        dcat:mediaType "text/html" .
     
    <http://www.nlgenome.nl> a foaf:Organization;
        foaf:name "The Genome of the Netherlands"@en.
     
    <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics/goNlSvR5/html-metadataID> a <http://edamontology.org/data_0842>;
        dct:identifier "html-metadataID".
```
### Data record metadata layer

Ontology     | Term name              | Required/Optional | Description
---          | ---                    | ---               | ---
DC terms     | dct:title              | `Required`        | Name of the datarecord with the language tag
|            | dct:conformsTo         | Optional          | The specification of the datarecord metadata schema (ShEx)
|            | dct:license            | `Required`        | Link to the license description
|            | dct:hasVersion         | `Required`        | Version of the datarecord
|            | dct:issued             | Optional          | Created date of the rml mappings
|            | dct:modified           | Optional          | Last modified date of the rml mappings
|            | dct:rights             | Optional          | 
|            | dct:description        | Optional          | Description of the description with the language tag
|            | dct:accessRights       | Optional          | Description of the access rights, see [Access rights rdf model](#access-rights-rdf-model)
|            | dct:isPartOf           | `Required`        | Relation to the parent metadata.
FDP ontology | fdp:metadataIdentifier | `Required`        | Identifier of the metadata entry. Define new sub property ‘metadataID’ for dct:identifier 
|            | fdp:metadataIssued     | `Required`        | Created date of the metadata entry
|            | fdp:metadataModified   | `Required`        | Last modified date of the metadata entry
|            | fdp:rmlMapping         | `Required`        | Link to the generic rml mapping
|            | fdp:rmlInputSource     | Optional          | Distribution used to generate RDF
RDF Schema   | rdfs:label             | Optional          | Name of the data datarecord with the language tag

An example of data record metadata
```ttl
@prefix fdp: <http://rdf.biosemantics.org/ontologies/fdp-o#> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix lang: <http://id.loc.gov/vocabulary/iso639-1/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

<> dct:title "An example data record metadata" ;
    rdfs:label "An example data record metadata" ;
    <http://rdf.biosemantics.org/ontologies/fdp-o#metadataIssued> "2016-10-27"^^xsd:date ;
    <http://rdf.biosemantics.org/ontologies/fdp-o#metadataIdentifier> <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/dataRecord-metadataID> ;
    <http://rdf.biosemantics.org/ontologies/fdp-o#metadataModified> "2016-10-27"^^xsd:date ;
    a fdp:DataRecord ; 
    dct:identifier "dataRecord" ;
    dct:language lang:en  ;
    dct:hasVersion "1.0" ;
    dct:publisher   <http://dtls.nl> ;
    fdp:rmlMapping <https://git.lumc.nl/biosemantics/ring14-fdp-metadata/raw/bd01b84fb792ae3860fdda646e9cb96a1a11205c/rml/biobank/RING_14_biobank_mapping.ttl> ;
    fdp:rmlInputSource <http://localhost.com/an-example-distribution>  .

<http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/dataRecord-metadataID> a <http://purl.org/spar/datacite/ResourceIdentifier> ;
    dct:identifier "dataRecord-metadataID" .

<http://dtls.nl> a foaf:Organization;
    foaf:name "DTLS"@en.
```

## Access rights rdf model
In the current implementation we use [WebAccessControl Vocabulary](https://www.w3.org/wiki/WebAccessControl/Vocabulary) to describe the access restriction of a resource. In the current of the FDP we are using this model to describe the access restriction of metadata, in future we would like to extend this model to describe the access restriction of `dcat:downloadUrl` and `dcat:accessUrl` of the dataset.    

An example of access rights.
```ttl
@prefix dcat: <http://www.w3.org/ns/dcat#> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix acl: <http://www.w3.org/ns/auth/acl#> .
     
<http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics/goNlSvR5/html> 
    a dcat:Distribution ;
        dct:accessRights <http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics/goNlSvR5/html/accessRights> .

<http://dev-vm.fair-dtls.surf-hosted.nl:8082/fdp/comparativeGenomics/goNlSvR5/html/accessRights> a dct:RightsStatement ;
    dct:description "This resource has access restriction";
dct:isPartOf <http://biosemantics.humgen.nl/groupAuthorization>.

<http://biosemantics.humgen.nl/groupAuthorization> a acl:Authorization;
        acl:mode acl:Read;
        acl:agent <http://orcid.org/0000-0002-1215-167X>.
```
The figure below illustrate the Access rights rdf model  

<p align="center"> 
     <img src="https://github.com/DTL-FAIRData/FAIRDataPoint/blob/wiki/FDP_access_control_dcterms_based.svg">
</p> 
<p align="center"> Fig. 4 - Access rights model</p>
