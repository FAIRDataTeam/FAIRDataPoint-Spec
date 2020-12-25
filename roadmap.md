# FAIR Datapoint Specification Roadmap
This document aims to lay out the medium to long-term future plans for the FDP specification.

## [Metadata of metadata]
The current (meta)data model mixes metadata (about the data) and 'metadata metadata' (metadata about the metadata: provenance, etc.). To separate these layers, a distinct description of a layer's metadata could be added.

## [Standardized navigational model]
The current (meta)data model uses [DCAT](https://www.w3.org/TR/vocab-dcat/) with some custom additions ([re3data](https://www.re3data.org/) etc.). To facilitate a broader uptake by existing client libraries, a standardized navigational model ([LDP](https://www.w3.org/TR/ldp/), [Hydra](http://www.hydra-cg.com/), etc.) could be considered.

## [Conditions of use]
To communicate to a human user what the conditions are for using the (meta)data, or to allow the FAIR Data Point to act in a Personal Health Train environment for automated reasoning, a clear description of conditions of use should be made available.

## [FAIR-Metrics]
To comply with the [FAIR Metrics](http://fairmetrics.org/), the (meta)data model should provide information in a way the evaluator implementations can understand.

## [Versioning]
To make a distinction between versions (of a dataset), there needs to be additional metadata in (or more fundamental changes to) the model. Both the [DCAT revised edition](https://w3c.github.io/dxwg/dcat/#dataset-versions) specification and the [DXWG wiki on versioning](https://github.com/w3c/dxwg/wiki/Dataset-%28and-other-DCAT%29-versioning) contain sections on versioning that could be followed.

## [Custom metadata extensions]
Currently the (meta)data model defines a minimal set of properties to describe the repository and its content. User-defined (extensions to the existing) properties are not considered. To allow for user extensions, the model could provide (among other things) standard mechanisms for describing an extension.

[Metadata of metadata]: https://github.com/FAIRDataTeam/FAIRDataPoint-Spec/issues/7
[Standardized navigational model]: https://github.com/FAIRDataTeam/FAIRDataPoint-Spec/issues/8
[Conditions of use]: https://github.com/FAIRDataTeam/FAIRDataPoint-Spec/issues/9
[FAIR-Metrics]: https://github.com/FAIRDataTeam/FAIRDataPoint-Spec/issues/10
[Versioning]: https://github.com/FAIRDataTeam/FAIRDataPoint-Spec/issues/11
[Custom metadata extensions]: https://github.com/FAIRDataTeam/FAIRDataPoint-Spec/issues/12
