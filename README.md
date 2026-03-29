[![License](https://img.shields.io/badge/license-Apache%202.0-brightgreen.svg)](LICENSE)
[![Website](https://img.shields.io/badge/https://-cyclonedx.org-blue.svg)](https://cyclonedx.org/)
[![Slack Invite](https://img.shields.io/badge/Slack-Join-blue?logo=slack&labelColor=393939)](https://cyclonedx.org/slack/invite)
[![Group Discussion](https://img.shields.io/badge/discussion-groups.io-blue.svg)](https://groups.io/g/CycloneDX)
[![Twitter](https://img.shields.io/twitter/url/http/shields.io.svg?style=social&label=Follow)](https://twitter.com/CycloneDX_Spec)
[![ECMA TC54](https://img.shields.io/badge/ECMA-TC54-FC7C00?labelColor=404040)](https://tc54.org)
[![ECMA TC54](https://img.shields.io/badge/ECMA-TC54--TG1-FC7C00?labelColor=404040)](https://ecma-international.org/task-groups/tc54-tg1/)

# CycloneDX Transparency Exchange API Reference Implementation

This repository contains the implementation code, supporting tooling, and
operational documentation for the CycloneDX Transparency Exchange API (TEA).
It is the home of the Rust server runtime in `tea-server/`, the Dagger
pipeline, local validation and generation tooling, and implementation-focused
documentation.

The normative TEA specification continues to live in the upstream specification
repository:

- https://github.com/CycloneDX/transparency-exchange-api

If you want to change the standard itself, open that change upstream. If you
want runnable code, deployment assets, validation tooling, or implementation
hardening, this repository is the right place.

![Transparency Exchange API Logo](images/tealogo.png "Transparency Exchange API Logo")

## Repository Scope

- `tea-server/`: Rust HTTP and gRPC reference server
- `dagger/`: build, conformance, and release automation
- `tools/`: validation and document-generation helpers used by the implementation
- `proto/` and `schemas/`: implementation-tracked TEA contracts consumed by the runtime and tooling
- `docs/` and `doc/`: architecture, evidence, integration, and implementation notes

## Status of This Repository

- Focus: executable reference implementation and integration tooling
- Consumer APIs: actively implemented
- Publisher APIs: partial and evolving
- Specification authority: upstream repository and the ECMA TC54 working process

## Relationship to the Specification

The upstream TEA specification repository remains the canonical place for:

- normative contract changes
- standard-language documentation
- schema and protocol review intended for standardization

This implementation repository tracks and exercises those contracts in code.
When spec and implementation changes need to move separately, the spec-facing
subset is proposed upstream and the implementation-specific work stays here.

## Key References

- [Upstream specification repository](https://github.com/CycloneDX/transparency-exchange-api)
- [Discovery overview](discovery/readme.md)
- [Authentication and authorization overview](auth/readme.md)
- [TEA Product Release](tea-product/tea-product-release.md)
- [TEA Component](tea-component/tea-component.md)
- [TEA Collection](tea-collection/tea-collection.md)
- [TEA Server](tea-server/README.md)

## Implementation Highlights

- Rust TEA server with public reads and authenticated writes
- Discovery, consumer, and partial publisher flows exercised end to end
- Sigstore, SLSA, and evidence-oriented supply chain automation
- Dagger-based CI and release tooling
- Protobuf and JSON-schema based contract handling for runtime and tooling

## Artefacts available of the API

The Transparency Exchange API (TEA) supports publication and retrieval of a set of transparency exchange artefacts. The API itself is not restricting the types of the artefacts published. A few examples:

### *xBOM

Bill of materials for any type of component and service are supported. This includes, but is not limited to, SBOM, HBOM, AI/ML-BOM, SaaSBOM, and CBOM. The API provides a BOM format agnostic way of publishing, searching, and retrieval of xBOM artefacts. 

### CDXA

Standards and requirements along with attestations to those standards and requirements are captured and supported by CycloneDX Attestations (CDXA). Much like xBOM, these are supply chain artefacts that are captured allowing for consistent publishing, searching, and retrieval.

### VDR/VEX

Vulnerability Disclosure Reports (VDR) and Vulnerability Exploitability eXchange (VEX) are supported artefact types. Like the xBOM element, the VDR/VEX support is format agnostic. However, CSAF has its own distribution requirements that may not be compatible with APIs. Therefore, the initial focus will be on CycloneDX (VDR and VEX) and OpenVEX.

### CLE

Product lifecycle events are communicated through the
[ECMA-428 Common Lifecycle Enumeration standard](https://ecma-international.org/publications-and-standards/standards/ecma-428/).
This includes product rebranding, repackaging, mergers and acquisitions, and product milestone events such as end-of-life and end-of-support.

Inclusion of CLE is optional and it may be introduced on the following levels:

- TEA Product
- TEA Component
- TEA Product Release
- TEA Component Release

If CLE is included, it is the responsibility of the TEA implementation to ensure consistency of
CLE events across the TEA Product and its releases and similarly across the TEA Component and its releases.

## Insights

Much of the focus on Software Transparency from the U.S. Government and others center around the
concept of “full transparency”. Consumers often need to ingest, process, and analyze SBOMs or
VEXs just to be able to answer simple questions such as:

- Do any of my licensed products from Vendor A use Apache Struts?
- Are any of my licensed products from Vendor A vulnerable to log4shell and is there any action I need to take?

Insights allows for “limited transparency” that can be asked and answered using an expression language
that can be tightly scoped or outcome-driven. Insights also removes the complexities of BOM format 
onversion away from the consumers. An object model derived from CycloneDX will be an integral part of
this API, since the objects within CycloneDX are self-contained (thus API friendly) and the specification
supports all the necessary xBOM types along with CDXA.

Insights will be integrated into the API after the 1.0 release.

## AI/ML BOM Profiles and Deprecation Support

The CycloneDX specification and the Transparency Exchange API (TEA) standard incorporate both AI/ML BOM profiles and mechanisms for managing deprecation, primarily within the core schema and API structure.

### AI/ML BOM Profiles

Machine Learning Bill of Materials (ML-BOM) support was introduced in CycloneDX v1.5 and enhanced in v1.6 and v1.7, providing detailed fields for AI and ML components.

**Component Type**: A new machine-learning-model component type was added to the schema.

**Detailed Metadata**: The schema allows documentation of various ML-specific metadata under this component type, including:

- Provenance: Information on training datasets, their sources, and integrity.
- Training Methodology: Details on the training process, parameters, and environmental factors like energy consumption and CO2 emissions.
- Intended Use and Limitations: Documentation of the model's intended use, known limitations, biases, and ethical considerations.
- Model Card Reference: The externalReferences field can use the model-card type to link to comprehensive model documentation.
- Architecture: Fields for describing the model architecture, parameters, and versioning.

**TEA Integration**: The Transparency Exchange API supports the exchange of all "xBOM" types, including AI/ML-BOMs, as format-agnostic artifacts, enabling automated discovery and retrieval of these specific BOMs.

### Deprecation Primitives/API/Schema

CycloneDX manages deprecation within its core schema evolution and API design, rather than as a separate "deprecation primitive" object.

**Schema Evolution**: The specification itself evolves, and fields are explicitly marked as deprecated within the JSON and XML schemas.

**Version Management**: The API and standard rely on versioning (e.g., v1.5, v1.6, v1.7). Future major versions, such as CycloneDX 2.0, will focus on completely removing deprecated fields to streamline the standard and tools.

**Machine-Readable Release Notes**: The standard supports machine-readable release notes, which often include explicit deprecation notices for components or APIs. This allows tools to automatically identify and flag components that are no longer supported.

**API Design**: The TEA is designed to handle different versions of BOMs and related artifacts. While the API standard is in Beta, it focuses on format-agnostic exchange and relies on the core CycloneDX specification's built-in methods for handling component lifecycles and deprecation through versioning and metadata.

For more details on the AI/ML BOM capabilities, refer to the Machine Learning Bill of Materials (ML-BOM) documentation. For information on the specification's evolution and deprecation markers, the CycloneDX specification GitHub repository is the primary source.

## Presentations and videos

- You can find presentations in the repository in the [Presentations](/presentations) directory
- Our biweekly meetings are available on [YouTube playlist: Project Koala](https://www.youtube.com/playlist?list=PLqjEqUxHjy1XtSzGYL7Dj_WJbiLu_ty58)
- KoalaCon 2024 - an introduction to the project - can be [viewed on YouTube](https://youtu.be/NStzYW4WnEE?si=ihLirpGVjHc7K4bL)

## Contributors

Contributors are listed in the [Contributors](contributors.md) file.

## Terminology

- API: Application programming interface
- Authorization (authz): Which products/components that a user has the right to access
- Authentication (authn): Credentials to get authorization
- Collection: A set of artefacts representing a version of a product
- Product: An item sold or delivered under one name
- Product variant: A variant of a product
- Version:

![Project Koala Logo](images/Project-Koala.svg "Project Koala Logo")

## Enterprise-Grade Features

This repository has been elevated to enterprise grade with the following security-first, evidence-native enhancements:

- **Hermetic Dependency Governance**: Two-lane CI with connected ingestion and airgapped builds, governed artifacts, recursive closures enforced by OPA.
- **Conformance Automation**: Dagger pipeline with OpenAPI validation, bidirectional security checks (mTLS), evidence collection.
- **Bidirectional Security**: API spec requires mutual TLS for client-server authentication, encrypted channels, JWT with signatures.
- **Evidence Collection**: SBOM generation (CycloneDX), SLSA provenance, attestations, Sigstore/cosign signing.
- **QA Processes**: golangci-lint, test coverage (Codecov), govulncheck, mutation testing (go-mutesting).
- **High-End Documentation**: Next.js site with Scalar API reference, architecture guides (DDD/Clean/Hexagonal/Three Spaces), evidence collection docs.
- **Stabilization**: Automated releases via GoReleaser, semantic versioning, buf schema management for Protobuf lifecycle.

All aligned with OWASP, CycloneDX, ECMA TC54 principles for long-lived, evolvable systems.

## Previous work

- [The CycloneDX BOM Exchange API](/api/bomexchangeapi.md)
   Implemented in the [CycloneDX BOM Repo Server](https://github.com/CycloneDX/cyclonedx-bom-repo-server)
