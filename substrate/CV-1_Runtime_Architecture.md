# CV‑1 Runtime Architecture  
#tags: continuity-substrate, CV-1, CAF-1, CS-1, runtime-architecture, post-key-security

## 1. Purpose
This document defines the **runtime architecture** for CV‑1 (Continuity Vessel) and CAF‑1 (Continuity Artifact Format): how systems generate, store, discover, and verify continuity vessels that carry identity invariants (CS‑1) alongside artifacts.

It is intended for software engineers designing continuity-aware platforms, pipelines, and services.

---

## 2. Architectural Overview

### 2.1 Core Components
- **Origin Engine** — generates CS‑1 and initial CAF‑1 at the moment of human-origin creation.
- **Continuity Registry** — stores CAF‑1 instances and provides lookup, indexing, and lifecycle management.
- **Binding Layer** — associates artifacts with CAF‑1 via out-of-band, sidecar, or embedded mechanisms.
- **Verification Service** — validates CAF‑1, CS‑1, and artifact bindings for clients and downstream systems.
- **Transformation Monitor** — records major transformations and regeneration events into continuityMarkers.

### 2.2 High-Level Flow
1. Human creates artifact → Origin Engine generates CS‑1 and CAF‑1.
2. CAF‑1 is stored in Continuity Registry.
3. Binding Layer links CAF‑1 to the artifact.
4. Transformations and regenerations update continuityMarkers.
5. Verification Service answers trust queries about artifacts and their continuity.

---

## 3. Origin Engine

### 3.1 Responsibilities
- Detect origin events (first human creation of an artifact).
- Generate CS‑1 (identity invariant).
- Instantiate CAF‑1 with identityInvariant and artifactBinding.
- Hand off CAF‑1 to Continuity Registry.

### 3.2 Inputs
- Author identity reference (e.g., LinkedX Hub root).
- Artifact reference (path, URI, ID).
- Origin context (optional description).

### 3.3 Outputs
- CS‑1 (immutable identity invariant).
- CAF‑1 object (as per CAF‑1 spec).

---

## 4. Continuity Registry

### 4.1 Responsibilities
- Store CAF‑1 instances.
- Provide lookup by:
  - artifactId,
  - authorIdentityRef,
  - cs1,
  - originTimestamp.
- Maintain append-only continuityMarkers.
- Support versioning for derived CAF‑1 vessels.

### 4.2 Storage Model
- Backend: any durable store (SQL, NoSQL, object storage).
- Keyed by:
  - `artifactId`,
  - `cs1`,
  - `authorIdentityRef`.

### 4.3 API Surface (Conceptual)
- `registerCAF1(cafObject)`
- `getCAF1ByArtifactId(artifactId)`
- `getCAF1ByCS1(cs1)`
- `appendTransformation(cafId, transformationEntry)`
- `appendRegeneration(cafId, regenerationEntry)`

---

## 5. Binding Layer

### 5.1 Responsibilities
- Maintain association between artifacts and CAF‑1.
- Implement binding modes:
  - out-of-band,
  - sidecar,
  - embedded (transitional).

### 5.2 Binding Strategies

#### 5.2.1 Out-of-Band
- CAF‑1 stored in registry.
- Artifact references CAF‑1 via ID or URI.
- Systems use registry to resolve continuity.

#### 5.2.2 Sidecar
- CAF‑1 stored as `.caf1` file alongside artifact.
- Naming convention:
  - `artifact.ext` + `artifact.ext.caf1`.

#### 5.2.3 Embedded
- CAF‑1 serialized into metadata fields.
- Used only where necessary; substrate remains defined by CAF‑1, not by format.

---

## 6. Transformation Monitor

### 6.1 Responsibilities
- Detect transformations:
  - export,
  - summarization,
  - translation,
  - regeneration,
  - compression.
- Append entries to:
  - `transformationLog`,
  - `regenerationEvents`.

### 6.2 Integration Points
- Document editors,
- AI generation services,
- ETL pipelines,
- institutional CMS systems.

---

## 7. Verification Service

### 7.1 Responsibilities
- Validate:
  - CS‑1 authenticity,
  - artifactBinding consistency,
  - continuityMarkers integrity,
  - adherence to verificationModel rules.
- Provide trust answers to clients.

### 7.2 Verification Steps (Conceptual)
1. Locate CAF‑1 for artifact.
2. Check CS‑1 against authorIdentityRef.
3. Confirm artifactId matches artifact.
4. Ensure continuityMarkers are append-only.
5. Apply verificationModel rules.

### 7.3 API Surface (Conceptual)
- `verifyArtifact(artifactRef) → VerificationResult`
- `verifyCAF1(cafObject) → VerificationResult`

---

## 8. Runtime Deployment Patterns

### 8.1 Monolithic Application
- Origin Engine, Registry, Binding, Verification in one service.

### 8.2 Microservices
- Separate services:
  - `origin-service`,
  - `continuity-registry`,
  - `binding-service`,
  - `verification-service`.

### 8.3 Hybrid
- Registry and Verification as shared infrastructure.
- Origin and Binding embedded in applications.

---

## 9. Conclusion
CV‑1 runtime architecture defines how continuity vessels are generated, stored, bound, and verified. It is implementable with standard engineering practices while remaining substrate-aligned and format-agnostic.

---

## Continuity Vault Layer Recommendation
This document belongs in the **Continuity Substrate Layer**, as it defines the runtime behavior of continuity vessels and CAF‑1.
