# FAIR Data Point metadata specification

The FAIR Data Point's metadata layered approach is composed of five layers, namely, the FAIR Data Point itself, the collection of datasets, each one of the offered datasets, the datasets' distribution and the structure and semantics of each of dataset.

For an overview of the FDP motivation and design, see [README.md](README.md).

# Version
The specification is currently at version [0.1.0](/../../releases/tag/v0.1.0).

Table of contents
===
- [FAIR Data Point metadata layer](#fair-data-point-metadata-layer)
- [Catalog, Dataset and Distribution metadata layers](#catalog-dataset-and-distribution-metadata-layers)
  - [Catalog metadata layer](#catalog-metadata-layer)
  - [Dataset metadata layer](#dataset-metadata-layer)
  - [Distribution metadata layer](#distribution-metadata-layer)
- [Access rights rdf model](#access-rights-rdf-model)

## FAIR Data Point metadata layer

The FAIR Data Point metadata contains information about the FDP itself and its governing authority. The FAIR Data Point metadata content is based on the [RE3Data Schema](http://www.re3data.org/schema).

FDP metadata content table 

Ontology     | Term name                | Datatype   | Required/Optional | Description
------------ | ------------------------ | ---------- | ----------------- | -----------
RDF          | rdf:type                 | IRI        | `Required`        | Required to be of type `r3d:Repository`
DC terms     | dct:title                | String     | `Required`        | Name of the repository with the language tag
|            | dct:hasVersion           | String     | `Required`        | Version of the repository
|            | dct:description          | String     | Optional          | Description of the repository with  the language tag
|            | dct:publisher            | IRI        | `Required`        | Organisation(s) responsible for the repository
|            | dct:language             | IRI        | Optional          | 
|            | dct:license              | IRI        | Optional          | 
|            | dct:conformsTo           | IRI        | Optional          | The specification of the repository metadata schema (for example ShEx)
|            | dct:rights               | IRI        | Optional          | 
|            | dct:references           | IRI        | Optional          | Reference to documentation (API or otherwise).
|            | dct:accessRights         | IRI        | Optional          | Description of the access rights, see [Access rights rdf model](#access-rights-rdf-model)
FDP ontology | fdp:metadataIdentifier   | IRI        | `Required`        | Identifier of the metadata entry. Define new sub property ‘metadataID’ for dct:identifier 
|            | fdp:metadataIssued       | DateTime   | `Required`        | Created date of the metadata entry
|            | fdp:metadataModified     | DateTime   | `Required`        | Last modified date of the metadata entry
RDF Schema   | rdfs:label               | String     | Optional          | Name of the repository with the language tag 
RE3Data      | r3d:institution          | IRI        | Optional          | 
|            | r3d:startDate            | DateTime   | Optional          | Release date of the repository
|            | r3d:lastUpdate           | DateTime   | Optional          | Last update timestamp of the repository
|            | r3d:dataCatalog          | IRI        | `Required`        | List of catalog metadata URLs
|            | r3d:country              | IRI        | Optional          |  
|            | r3d:repositoryIdentifier | IRI        | `Required`        | Identifier of the repository.

An example of FDP metadata

```ttl
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix fdp: <http://rdf.biosemantics.org/ontologies/fdp-o#> .
@prefix lang: <http://id.loc.gov/vocabulary/iso639-1/> .
@prefix r3d: <http://www.re3data.org/schema/3-0#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<http://136.243.4.200:8087/fdp> a r3d:Repository;
  dcterms:accessRights <http://136.243.4.200:8087/fdp/accessRights>;
  dcterms:conformsTo <http://rdf.biosemantics.org/fdp/shex/fdpMetadata>;
  dcterms:description "This is a prototype FDP for hosting research and student projects datasets";
  dcterms:hasVersion "1.0";
  dcterms:language lang:en;
  dcterms:license <http://rdflicense.appspot.com/rdflicense/cc-by-nc-nd3.0>;
  dcterms:publisher <http://biosemantics.org>;
  dcterms:title "FDP of biosemantics group";
  fdp:metadataIdentifier <http://purl.org/biosemantics-lumc/fdp>;
  fdp:metadataIssued "2017-05-23T09:43:15.57Z"^^xsd:dateTime;
  fdp:metadataModified "2018-08-20T13:09:55"^^xsd:dateTime;
  r3d:dataCatalog <http://136.243.4.200:8087/fdp/catalog/Biosamples>, <http://136.243.4.200:8087/fdp/catalog/multiomics>, 
       <http://136.243.4.200:8087/fdp/catalog/textmining>;
  r3d:institutionCountry <http://lexvo.org/id/iso3166/NL>;
  r3d:repositoryIdentifier <http://136.243.4.200:8087/fdp#repositoryID>;
  rdfs:label "FDP of biosemantics group";
  dcterms:references <http://136.243.4.200:8087/fdp/swagger-ui.html> .

<http://purl.org/biosemantics-lumc/fdp> a <http://purl.org/spar/datacite/ResourceIdentifier>;
  dcterms:identifier "fdp" .

<http://biosemantics.org> a <http://xmlns.com/foaf/0.1/Organization>;
  <http://xmlns.com/foaf/0.1/name> "Biosemantic group" .

<http://136.243.4.200:8087/fdp/accessRights> a dcterms:RightsStatement;
  dcterms:description "This resource has no access restriction" .

<http://136.243.4.200:8087/fdp#repositoryID> a <http://purl.org/spar/datacite/Identifier>;
  dcterms:identifier "176c810f-504a-421a-903b-742a70c2806a" .
```

## Catalog, Dataset and Distribution metadata layers

For the representation of the catalog of datasets, each one of the offered datasets and their distributions, we adopt as basis the [W3C's Data Catalog Vocabulary (DCAT)](http://www.w3.org/TR/vocab-dcat/). DCAT defines three main classes:
* dcat:Catalog: defines the catalog, i.e., the collection of datasets;
* dcat:Dataset: represents an individual dataset in the collection;
* dcat:Distribution: represent an accessible form of a dataset, e.g., a downloadable file or a web service that gives access to the data.

### Catalog metadata layer

Catalog metadata content table 

Ontology     | Term name              | DataType   | Required/Optional | Description
--------     | ---------              | ---------- | ----------------- | -----------
RDF          | rdf:type               | IRI        | `Required`        | Required to be of type `dcat:Catalog`
DC terms     | dct:title              | String     | `Required`        | Name of the catalog with the language tag
|            | dct:hasVersion         | String     | `Required`        | Version of the catalog
|            | dct:publisher          | IRI        | `Required`        | Organisation(s)  or Persons(s) responsible for the catalog
|            | dct:description        | String     | Optional          | Description of the catalog with the language tag
|            | dct:language           | IRI        | Optional          | 
|            | dct:license            | IRI        | Optional          | 
|            | dct:issued             | DateTime   | Optional          | Created date of the catalog entry
|            | dct:modified           | DateTime   | Optional          | Last modified date of the catalog entry
|            | dct:conformsTo         | IRI        | Optional          | The specification of the catalog metadata schema (for example ShEx)
|            | dct:rights             | IRI        | Optional          | 
|            | dct:accessRights       | IRI        | Optional          | Description of the access rights, see [Access rights rdf model](#access-rights-rdf-model)
|            | dct:isPartOf           | IRI        | `Required`        | Relation to the parent metadata.
FDP ontology | fdp:metadataIdentifier | IRI        | `Required`        | Identifier of the metadata entry. Define new sub property ‘metadataID’ for dct:identifier 
|            | fdp:metadataIssued     | DateTime   | `Required`        | Created date of the metadata entry
|            | fdp:metadataModified   | DateTime   | `Required`        | Last modified date of the metadata entry
RDF Schema   | rdfs:label             | String     | Optional          | Name of the catalog with the language tag
FOAF         | foaf:homepage          | IRI        | Optional          | 
DCAT         | dcat:dataset           | IRI        | `Required`        | List of dataset URLs
|            | dcat:themeTaxonomy     | IRI        | `Required`        | List of taxonomy URLs
 
An example of catalog metadata.

```ttl
@prefix dcat: <http://www.w3.org/ns/dcat#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix fdp: <http://rdf.biosemantics.org/ontologies/fdp-o#> .
@prefix lang: <http://id.loc.gov/vocabulary/iso639-1/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<http://136.243.4.200:8087/fdp/catalog/textmining> a dcat:Catalog;
  dcterms:accessRights <http://136.243.4.200:8087/fdp/catalog/textmining#accessRights>;
  dcterms:conformsTo <https://www.purl.org/fairtools/fdp/schema/0.1/catalogMetadata>;
  dcterms:description "Catalog for describing textmining datasets";
  dcterms:hasVersion "1.0";
  dcterms:isPartOf <http://136.243.4.200:8087/fdp>;
  dcterms:language lang:en;
  dcterms:license <http://rdflicense.appspot.com/rdflicense/cc-by-nc-nd3.0>;
  dcterms:publisher <http://biosemantics.org>;
  dcterms:title "Catalog for textmining datasets";
  fdp:metadataIdentifier <http://purl.org/biosemantics-lumc/fdp/catalog/textmining>;
  fdp:metadataIssued "2018-03-20T10:20:37.08Z"^^xsd:dateTime;
  fdp:metadataModified "2018-08-20T13:09:55"^^xsd:dateTime;
  rdfs:label "Catalog for textmining datasets";
  dcat:dataset <http://136.243.4.200:8087/fdp/dataset/gene_disease_association>;
  dcat:themeTaxonomy <http://dbpedia.org/resource/Text_mining>, <http://edamontology.org/topic_0218> .

<http://purl.org/biosemantics-lumc/fdp/catalog/textmining> a <http://purl.org/spar/datacite/ResourceIdentifier>;
  dcterms:identifier "textmining" .

<http://biosemantics.org> a <http://xmlns.com/foaf/0.1/Agent>;
  <http://xmlns.com/foaf/0.1/name> "Biosemantic group" .

<http://136.243.4.200:8087/fdp/catalog/textmining#accessRights> a dcterms:RightsStatement;
  dcterms:description "This resource has no access restriction" .
```

### Dataset metadata layer

Ontology     | Term name              | DataType   | Required/Optional | Description
---          | ---                    | --------   |---                | ---
RDF          | rdf:type               | IRI        | `Required`        | Required to be of type `dcat:Dataset`
DC terms     | dct:title              | String     | `Required`        | Name of the dataset with the language tag 
|            | dct:publisher          | IRI        | `Required`        | Organisation(s)  or Persons(s) responsible for the dataset
|            | dct:hasVersion         | String     | `Required`        | Version of the dataset 
|            | dct:description        | String     | Optional          | Description of the dataset with the language tag
|            | dct:conformsTo         | IRI        | Optional          | The specification of the dataset metadata schema (for example ShEx)
|            | dct:issued             | DateTime   | Optional          | Created date of the dataset entry
|            | dct:modified           | DateTime   | Optional          | Last modified date of the dataset entry 
|            | dct:language           | IRI        | Optional          | 
|            | dct:license            | IRI        | Optional          | 
|            | dct:rights             | IRI        | Optional          | 
|            | dct:accessRights       | IRI        | Optional          | Description of the access rights, see [Access rights rdf model](#access-rights-rdf-model)
|            | dct:isPartOf           | IRI        | `Required`        | Relation to the parent metadata.
FDP ontology | fdp:metadataIdentifier | IRI        | `Required`        | Identifier of the metadata entry. Define new sub property ‘metadataID’ for dct:identifier 
|            | fdp:metadataIssued     | DateTime   | `Required`        | Created date of the metadata entry
|            | fdp:metadataModified   | DateTime   | `Required`        | Last modified date of the metadata entry
RDF Schema   | rdfs:label             | String     | Optional          | Name of the dataset with the language tag
DCAT         | dcat:distribution      | IRI        | `Required`        | List of distribution URLs
|            | dcat:theme             | IRI        | `Required`        | List of concepts that describe the dataset
|            | dcat:contactPoint      | IRI        | Optional          | 
|            | dcat:keyword           | String     | Optional          | Keyword(s) related to the dataset with the language tag 
|            | dcat:landingPage       | IRI        | Optional          | Home page of the dataset

An example of dataset metadata.

```ttl
@prefix dcat: <http://www.w3.org/ns/dcat#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix fdp: <http://rdf.biosemantics.org/ontologies/fdp-o#> .
@prefix lang: <http://id.loc.gov/vocabulary/iso639-1/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<http://136.243.4.200:8087/fdp/dataset/gene_disease_association> a dcat:Dataset;
  dcterms:accessRights <http://136.243.4.200:8087/fdp/dataset/gene_disease_association#accessRights>;
  dcterms:conformsTo <https://www.purl.org/fairtools/fdp/schema/0.1/datasetMetadata>;
  dcterms:description "High-throughput experimental methods such as medical sequencing and genome-wide association studies (GWAS) identify increasingly large numbers of potential relations between genetic variants and diseases. Both biological complexity (millions of potential gene-disease associations) and the accelerating rate of data production necessitate computational approaches to prioritize and rationalize potential gene-disease relations. Here, we use concept profile technology to expose from the biomedical literature both explicitly stated gene-disease relations (the explicitome) and a much larger set of implied gene-disease associations (the implicitome). Implicit relations are largely unknown to, or are even unintended by the original authors, but they vastly extend the reach of existing biomedical knowledge for identification and interpretation of gene-disease associations. The implicitome can be used in conjunction with experimental data resources to rationalize both known and novel associations. We demonstrate the usefulness of the implicitome by rationalizing known and novel gene-disease associations, including those from GWAS. To facilitate the re-use of implicit gene-disease associations, we publish our data in compliance with FAIR Data Publishing recommendations [https://www.force11.org/group/fairgroup] using nanopublications. An online tool (http://knowledge.bio) is available to explore established and potential gene-disease associations in the context of other biomedical relations.";
  dcterms:hasVersion "1.0";
  dcterms:isPartOf <http://136.243.4.200:8087/fdp/catalog/textmining>;
  dcterms:language lang:en;
  dcterms:license <http://rdflicense.appspot.com/rdflicense/cc-by-nc-nd3.0>;
  dcterms:publisher <http://biosemantics.org>;
  dcterms:title "Gene disease association (LUMC)";
  fdp:metadataIdentifier <http://purl.org/biosemantics-lumc/fdp/dataset/gene_disease_association>;
  fdp:metadataIssued "2018-03-20T10:30:18.662Z"^^xsd:dateTime;
  fdp:metadataModified "2018-08-20T13:09:55"^^xsd:dateTime;
  rdfs:label "Gene disease association (LUMC)";
  dcat:distribution <http://136.243.4.200:8087/fdp/distribution/gene_disease_association_csv_gzip>,
    <http://136.243.4.200:8087/fdp/distribution/gene_disease_association_html>, <http://136.243.4.200:8087/fdp/distribution/gene_disease_association_nquads_gzip>;
  dcat:keyword "GDA", "Gene disease association (LUMC)", "LWAS", "Text mining", "The Explicitome",
    "The Implicitome";
  dcat:theme <http://dbpedia.org/resource/Text_mining>, <http://semanticscience.org/resource/statistical-association> .

<http://purl.org/biosemantics-lumc/fdp/dataset/gene_disease_association> a <http://purl.org/spar/datacite/ResourceIdentifier>;
  dcterms:identifier "gene_disease_association" .

<http://biosemantics.org> a <http://xmlns.com/foaf/0.1/Agent>;
  <http://xmlns.com/foaf/0.1/name> "Biosemantic group" .

<http://136.243.4.200:8087/fdp/dataset/gene_disease_association#accessRights> a dcterms:RightsStatement;
  dcterms:description "This resource has no access restriction" .
```

### Distribution metadata layer

Ontology     | Term name              | DataType   | Required/Optional | Description
--------     | ---------              | --------   | ----------------  | -----------
RDF          | rdf:type               | IRI        | `Required`        | Required to be of type `dcat:Distribution`
DC terms     | dct:title              | String     | `Required`        | Name of the data distribution with the language tag
|            | dct:conformsTo         | IRI        | Optional          | The specification of the distribution metadata schema (for example ShEx)
|            | dct:license            | IRI        | `Required`        | Link to the license description
|            | dct:hasVersion         | String     | `Required`        | Version of the distribution
|            | dct:issued             | DateTime   | Optional          | Created date of the distribution entry
|            | dct:modified           | DateTime   | Optional          | Last modified date of the distribution entry
|            | dct:rights             | IRI        | Optional          | 
|            | dct:description        | String     | Optional          | Description of the description with the language tag
|            | dct:accessRights       | IRI        | Optional          | Description of the access rights, see [Access rights rdf model](#access-rights-rdf-model)
|            | dct:isPartOf           | IRI        | `Required`        | Relation to the parent metadata.
FDP ontology | fdp:metadataIdentifier | IRI        | `Required`        | Identifier of the metadata entry. Define new sub property ‘metadataID’ for dct:identifier 
|            | fdp:metadataIssued     | DateTime   | `Required`        | Created date of the metadata entry
|            | fdp:metadataModified   | DateTime   | `Required`        | Last modified date of the metadata entry
RDF Schema   | rdfs:label             | String     | Optional          | Name of the data distribution with the language tag
DCAT         | dcat:accessURL         | IRI        | `Required` (or dcat:downloadURL) | A landing page, feed, SPARQL endpoint or other type of resource that gives access to the distribution of the dataset
|            | dcat:downloadURL       | IRI        | `Required` (or dcat:accessURL)   | A file that contains the distribution of the dataset in a given format
|            | dcat:mediaType         | String     | `Required`        | The media type of the distribution
|            | dcat:format            | String     | Optional          | 
|            | dcat:byteSize          | Decimal    | Optional          | 
 


An example of distribution metadata.
```ttl
@prefix dcat: <http://www.w3.org/ns/dcat#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix fdp: <http://rdf.biosemantics.org/ontologies/fdp-o#> .
@prefix lang: <http://id.loc.gov/vocabulary/iso639-1/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

<http://136.243.4.200:8087/fdp/distribution/gene_disease_association_nquads_gzip>
  a dcat:Distribution;
  dcterms:accessRights <http://136.243.4.200:8087/fdp/distribution/gene_disease_association_nquads_gzip#accessRights>;
  dcterms:conformsTo <https://www.purl.org/fairtools/fdp/schema/0.1/distributionMetadata>;
  dcterms:description "The complete set of all ~204 million associations (explicit and implicit) as nanopublications. Each nanopublication asserts an association between a gene and a disease concept and the percentile rank of the match score.";
  dcterms:hasVersion "1.0";
  dcterms:isPartOf <http://136.243.4.200:8087/fdp/dataset/gene_disease_association>;
  dcterms:language lang:en;
  dcterms:license <http://rdflicense.appspot.com/rdflicense/cc-by-nc-nd3.0>;
  dcterms:publisher <http://biosemantics.org>;
  dcterms:title "Gene disease association (LUMC) nquads as gzip distribution";
  fdp:metadataIdentifier <http://purl.org/biosemantics-lumc/fdp/distribution/gene_disease_association_nquads_gzip>;
  fdp:metadataIssued "2018-03-20T10:40:17.677Z"^^xsd:dateTime;
  fdp:metadataModified "2018-08-20T13:09:55"^^xsd:dateTime;
  rdfs:label "Gene disease association (LUMC) nquads as gzip distribution";
  dcat:downloadURL <https://datadryad.org/bitstream/handle/10255/dryad.91060/gda-np.nq.gz?sequence=1>;
  dcat:mediaType "application/gzip" .

<http://purl.org/biosemantics-lumc/fdp/distribution/gene_disease_association_nquads_gzip>
  a <http://purl.org/spar/datacite/ResourceIdentifier>;
  dcterms:identifier "gene_disease_association_nquads_gzip" .

<http://biosemantics.org> a <http://xmlns.com/foaf/0.1/Agent>;
  <http://xmlns.com/foaf/0.1/name> "Biosemantic group" .

<http://136.243.4.200:8087/fdp/distribution/gene_disease_association_nquads_gzip#accessRights>
  a dcterms:RightsStatement;
  dcterms:description "This resource has no access restriction" .
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
