# CAF‑1 Specification  
# Continuity Artifact Format — Substrate-Level Vessel for Identity Continuity  
#tags: continuity-substrate, CV-1, CS-1, post-key-security, artifact-binding, identity-invariance

## 1. Purpose
CAF‑1 defines the substrate-level representation of a **Continuity Vessel (CV‑1)** that carries identity invariants (CS‑1) alongside any digital artifact, regardless of its file format. CAF‑1 does not replace existing formats (PDF, DOCX, HTML, email). Instead, it provides a **format-agnostic vessel** that preserves origin continuity, identity sovereignty, and accountability across all transformations, regenerations, and multi-agent workflows.

CAF‑1 is designed for implementation by software engineers building continuity-aware systems, identity-preserving pipelines, and post-key security infrastructures.

---

## 2. Core Principles

### 2.1 Format-Agnostic
CAF‑1 must operate independently of the artifact’s file format. It cannot rely on:
- embedded metadata,
- cryptographic signatures,
- watermarking,
- institutional publication systems.

CAF‑1 is a **parallel vessel**, not a format extension.

### 2.2 Immutable Identity Invariant
CAF‑1 carries **CS‑1**, a one-time identity invariant generated at the moment of human-origin creation. CS‑1 must:
- be immutable,
- be unclonable,
- be imitation-proof,
- survive all transformations of the artifact.

### 2.3 Artifact Binding Without Content Dependence
CAF‑1 binds to an artifact through a stable identifier (ArtifactID) but does not depend on the artifact’s content for identity continuity.

### 2.4 Transformation-Resilient
CAF‑1 must remain valid even when the artifact is:
- edited,
- summarized,
- translated,
- regenerated,
- compressed,
- re-rendered,
- redistributed.

### 2.5 Human-Origin Anchoring
CAF‑1 must preserve:
- origin identity,
- origin timestamp,
- origin context,
- sovereign accountability.

---

## 3. CAF‑1 Object Model

CAF‑1 is represented as a structured object. JSON is recommended for interoperability, but YAML or binary encodings are allowed.

### 3.1 CAF‑1 Schema (Canonical Form)

```json
{
  "cafVersion": "1.0",
  "identityInvariant": {
    "cs1": "string", 
    "authorIdentityRef": "string",
    "originTimestamp": "string",
    "originContext": "string"
  },
  "artifactBinding": {
    "artifactId": "string",
    "artifactType": "string",
    "artifactState": "string"
  },
  "continuityMarkers": {
    "transformationLog": ["string"],
    "regenerationEvents": ["string"]
  },
  "verificationModel": {
    "modelRef": "string",
    "validationRules": ["string"]
  }
}
```

### 3.2 Field Definitions

cafVersion

Version of the CAF‑1 specification. Enables forward compatibility.

identityInvariant

Carries the identity continuity layer.

cs1 — the one-time identity invariant signature.

authorIdentityRef — stable reference to the human identity (e.g., LinkedX Hub identity root).

originTimestamp — ISO 8601 timestamp of the origin event.

originContext — optional human-readable context (e.g., “Leadership memo on AI accountability”).


artifactBinding

Defines how CAF‑1 attaches to the artifact.

artifactId — stable identifier (hash, UUID, URI, or system-generated ID).

artifactType — e.g., “pdf”, “docx”, “html”, “email”, “text”, “ai-output”.

artifactState — optional descriptor (e.g., “original”, “exported”, “summarized”, “translated”).


continuityMarkers

Tracks major transformations without relying on provenance.

transformationLog — human or system-generated entries describing transformations.

regenerationEvents — entries describing AI or multi-agent regeneration.


verificationModel

Defines how a verifier checks CAF‑1 validity.

modelRef — reference to the continuity verification model.

validationRules — rules for verifying CS‑1, artifact binding, and continuity markers.


## 4. Binding Mechanisms
CAF‑1 supports three binding modes. Engineers may implement one or all.

### 4.1 Out-of-Band Binding (Preferred)
CAF‑1 exists as a separate .caf1 vessel file stored alongside the artifact.

Example:
memo.pdf
memo.caf1

### 4.2 Sidecar Binding
CAF‑1 is stored as a sidecar file with a naming convention.

Example:
memo.docx
memo.docx.caf1

### 4.3 Embedded Binding (Transitional)
CAF‑1 is embedded into custom metadata fields of existing formats.

Examples:
PDF XMP metadata
DOCX custom properties
HTML <meta> tags

This mode is transitional and not recommended for long-term substrate integrity.

## 5. Artifact Lifecycle with CAF‑1

### 5.1 Origin Event

Human creates artifact.
System generates CS‑1.
CAF‑1 is instantiated with identityInvariant and artifactBinding.

### 5.2 Transformation Event

When artifact changes:
CAF‑1 remains unchanged OR
CAF‑1 spawns a derived vessel referencing the parent.

### 5.3 Regeneration Event

AI-generated versions:
inherit CS‑1,
append regenerationEvents,
update artifactBinding.

### 5.4 Verification Event

A verifier checks:
CS‑1 validity,
artifactBinding consistency,
continuityMarkers integrity,
verificationModel rules.

## 6. Engineering Requirements

### 6.1 Deterministic ArtifactID

ArtifactID must be stable across systems. Recommended options:
SHA‑256 hash of canonicalized artifact bytes,
UUIDv5 derived from artifact content,
system-generated immutable ID.

### 6.2 CS‑1 Generation

CS‑1 must be:
generated only once per origin event,
immutable,
non-replayable,
non-transferable.

### 6.3 Storage Requirements

CAF‑1 may be stored:
in local filesystem,
in cloud object storage,
in distributed continuity registries,
in institutional continuity hubs.

### 6.4 Security Requirements
CAF‑1 must not:
rely on cryptographic keys,
rely on PKI,
rely on watermarking,
rely on provenance metadata.

CAF‑1 must:
preserve identity invariance,
survive all transformations,
remain format-agnostic.

## 7. Interoperability Requirements

### 7.1 Platform-Agnostic

CAF‑1 must work across:
Windows, macOS, Linux,
cloud platforms,
mobile devices,
enterprise systems.

### 7.2 Format-Agnostic

CAF‑1 must support:
PDF, DOCX, TXT, HTML,
JSON, CSV, XML,
images, audio, video,
AI-generated artifacts.

### 7.3 Multi-Agent Compatibility

CAF‑1 must remain valid in:
human-only workflows,
human + AI workflows,
multi-AI agent workflows,
institutional pipelines.

## 8. Example CAF‑1 Instance

```json
{
  "cafVersion": "1.0",
  "identityInvariant": {
    "cs1": "CS1-9f2a7c1e-immutable",
    "authorIdentityRef": "linkedx://joel-root",
    "originTimestamp": "2026-08-17T08:15:00Z",
    "originContext": "Leadership memo on AI accountability"
  },
  "artifactBinding": {
    "artifactId": "sha256:ab92f1c4...",
    "artifactType": "pdf",
    "artifactState": "exported"
  },
  "continuityMarkers": {
    "transformationLog": [
      "docx → pdf export",
      "pdf → ai-summary generation"
    ],
    "regenerationEvents": [
      "AI summary generated from original memo"
    ]
  },
  "verificationModel": {
    "modelRef": "continuity://v1",
    "validationRules": [
      "CS‑1 must match authorIdentityRef",
      "artifactId must match bound artifact",
      "continuityMarkers must be append-only"
    ]
  }
}
```

## 9. Conclusion
CAF‑1 provides a formal, implementable specification for binding identity invariants to artifacts in a way that survives transformation, regeneration, and institutional masking. It is the substrate vessel required for post-key security and continuity geometry.

CAF‑1 is ready for engineering implementation.

Continuity Vault Layer 

This document belongs in the Continuity Substrate Layer, as it defines the formal specification for identity-continuity vessels used across all artifact formats.
