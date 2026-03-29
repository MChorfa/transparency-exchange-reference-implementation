[![License](https://img.shields.io/badge/license-Apache%202.0-brightgreen.svg)](LICENSE)
[![Website](https://img.shields.io/badge/https://-cyclonedx.org-blue.svg)](https://cyclonedx.org/)
[![Slack Invite](https://img.shields.io/badge/Slack-Join-blue?logo=slack&labelColor=393939)](https://cyclonedx.org/slack/invite)
[![Group Discussion](https://img.shields.io/badge/discussion-groups.io-blue.svg)](https://groups.io/g/CycloneDX)

# CycloneDX Transparency Exchange API Reference Implementation

This repository contains the executable reference implementation and associated
tooling for the CycloneDX Transparency Exchange API (TEA). It is the home of
the Rust server runtime, implementation-side contract handling, CI/release
automation, and operational documentation.

The normative TEA specification remains upstream at:

- https://github.com/CycloneDX/transparency-exchange-api

If a change affects the standard itself, it belongs upstream. If it affects the
runtime, deployment assets, validation tooling, or implementation-focused docs,
it belongs here.

![Transparency Exchange API Logo](images/tealogo.png "Transparency Exchange API Logo")

## Repository Scope

- `tea-server/`: Rust HTTP and gRPC reference server
- `dagger/`: build, conformance, and release automation
- `tools/`: validation and document-generation helpers
- `proto/` and `schemas/`: implementation-tracked TEA contracts used by the runtime and tooling
- `docs/` and `doc/`: architecture, evidence, integration, and implementation notes

## Status

- Focus: executable reference implementation and integration tooling
- Consumer APIs: actively implemented
- Publisher APIs: partial and evolving
- Specification authority: upstream repository and the ECMA TC54 working process

## Getting Started

- Runtime setup and local execution: [tea-server/README.md](tea-server/README.md)
- Discovery overview: [discovery/readme.md](discovery/readme.md)
- Authentication and authorization notes: [auth/readme.md](auth/readme.md)
- Core TEA model docs: [tea-product](tea-product), [tea-component](tea-component), [tea-collection](tea-collection), [tea-artifact](tea-artifact)

## Contributing

- Spec changes go to the upstream specification repository: https://github.com/CycloneDX/transparency-exchange-api
- Implementation changes, runtime behavior, CI/release automation, and integration tooling go here
- If a change spans both spec and implementation, split it into two changesets: one upstream for the normative delta, one here for the runnable implementation work
- When changing implementation-tracked contracts in `proto/` or `schemas/`, keep the runtime and tooling updates in the same change so the repository stays executable

## Implementation Highlights

- Rust TEA server with public reads and authenticated writes
- Discovery, consumer, and partial publisher flows exercised end to end
- Supply-chain oriented evidence and signing integrations
- Dagger-based CI and release tooling
- Protobuf and JSON-schema based contract handling for runtime and tooling

## References

- [Upstream specification repository](https://github.com/CycloneDX/transparency-exchange-api)
- [TEA Server](tea-server/README.md)
- [Presentations](presentations)
- [Contributors](contributors.md)
