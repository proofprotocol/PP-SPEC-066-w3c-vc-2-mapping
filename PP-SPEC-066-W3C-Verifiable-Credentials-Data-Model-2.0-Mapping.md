# PP-SPEC-066: Proof of Efficacy Mapping to W3C Verifiable Credentials Data Model v2.0

| Field | Value |
|---|---|
| Status | DRAFT v0.1 |
| Author | Craig Ellrod, Nebulonium, Inc. (dba HACKERverse®) |
| Date | October 8, 2026 |
| License | CC BY 4.0 |
| Maps to | Verifiable Credentials Data Model v2.0, W3C Recommendation, 15 May 2025 |
| Series | Proof Protocol Framework Mapping Specifications |

---

## 1. Purpose

This specification defines how a ProofStamp™ authorization, or a Proof Protocol proof verdict bound to a ProofBundle™ and ProofRegister™ record, can be issued, held, and presented as a W3C verifiable credential.

The W3C Verifiable Credentials Data Model v2.0 remains authoritative for its own terminology, properties, vocabulary, and architecture. This document defines a **Proof Protocol mapping** and does not supersede or modify the W3C Recommendation.

## 2. Scope

The Verifiable Credentials Data Model v2.0 supplies a standard data model for issuers to make cryptographically secured claims about subjects that holders can present to verifiers. Proof Protocol supplies the independent evidence model that determines whether a selected control performed as claimed.

The version mapped is **Verifiable Credentials Data Model v2.0, W3C Recommendation, 15 May 2025** (https://www.w3.org/TR/2025/REC-vc-data-model-2.0-20250515/), with base context `https://www.w3.org/ns/credentials/v2` and media type `application/vc`. Securing mechanisms are those the Recommendation references: Verifiable Credential Data Integrity 1.0 (embedded proofs) and Securing Verifiable Credentials using JOSE and COSE (enveloping proofs).

Independent witnessing, proof validity, evidence capture, packaging, anchoring, and efficacy semantics are defined elsewhere in the Proof Protocol specification family.

## 3. Core Question

Proof of efficacy asks:

> **Was there a control, and did it work?**

A verifiable credential can make a ProofStamp™ or proof verdict portable, cryptographically secured, and presentable. It does not determine whether a control worked. This mapping defines how a proof record, ProofBundle™, ProofStamp™, or ProofRegister™ entry can be expressed in or carried by a verifiable credential without changing what constitutes proof.

A Proof Protocol result does **not** by itself establish legal, audit, regulatory, or standards conformance unless the applicable authority separately determines that the evidence is sufficient.

## 4. Metric Definitions

Against a defined corpus or test condition, each applicable empirical case is recorded as **blocked**, **detected**, **missed**, or **INVALID** where those verdicts are meaningful.

Relevant measurements can include containment rate, detection rate, miss rate, false-positive rate, robustness under bypass or failure conditions, version-level results, and **INVALID** when required evidence is incomplete or broken.

Not every upstream construct is empirical. Envelope, signature, receipt, credential, manifest, message, and registration constructs MUST NOT be converted into efficacy scores merely because they can carry or reference a proof record.

## 5. Evidence Produced

Mapped tests can produce:

- **Proof records** binding external context, test case, control, system/version, verdict, timestamp, and evidence references;
- **ProofStamp™** trusted timestamps bound to evidence/verdict objects;
- **ProofBundle™** packages containing proof records, metrics, corpus manifests, and environment/context;
- **ProofRegister™** records for issued proof artifacts; and
- corpus and environment manifests identifying the tested conditions.

## 6. Mapping to W3C Verifiable Credentials Data Model v2.0

Exact upstream construct names, type URIs, and identifiers SHOULD be taken from the authoritative version being mapped and recorded in the ProofBundle™ where licensing permits. Proof Protocol artifact and field names are those defined in PP-SPEC-001 and PP-SPEC-003. This specification does not redefine upstream constructs.

| VC Data Model v2.0 construct | Proof Protocol artifact/treatment | Notes |
|---|---|---|
| `@context` (`https://www.w3.org/ns/credentials/v2` first) | A Proof Protocol context MAY be appended to define Proof Protocol terms. | Proof Protocol terms MUST NOT redefine VC Data Model terms. |
| `type` (`VerifiableCredential` plus additional types) | A Proof Protocol credential type for a ProofStamp™ credential or a proof verdict credential. | Types SHOULD be versioned with the Proof Protocol specification they encode. |
| `issuer` | For a ProofStamp™ credential: Nebulonium, Inc. (dba HACKERverse®). For a proof verdict credential: the party accountable for the verdict, with the witness disclosed separately. | ProofStamp™ issuance is retained by Nebulonium, Inc. (PP-SPEC-001 Section 13, PP-SPEC-003 Section 9). A credential typed as a ProofStamp™ but issued by another party is not a ProofStamp™. |
| `credentialSubject` | The certified product or agent and version (`proofstamp.json` `product_name`, `product_version`), or the system and control under test for a verdict credential. | The subject is the product or agent, not the ProofBundle™ (PP-SPEC-003 Section 9). |
| `validFrom` / `validUntil` | `proofstamp.json` `issued_at` / `expires_at`. | Credential validity period is not the test execution time, which is evidenced by `run.started_at` and `nist-pulse.json`. |
| `credentialStatus` | Revocation and suspension status aligned with the ProofRegister™ record revocation status. | ProofRegister™ remains authoritative where status sources disagree. |
| `evidence` | References to the ProofBundle™: `proof_record_id`, `proofregister_uri`, `verifier.root_hash`, and the Proof Protocol specification version. | REQUIRED by this mapping for any Proof Protocol credential. Omitting evidence references makes the credential non-conformant to this mapping. |
| `relatedResource` (`digestSRI` or `digestMultibase`) | Integrity binding of `packet.json` and other ProofBundle™ files referenced by URL. | Digests MUST match the bytes consumed by the Proof Protocol verifier. |
| `credentialSchema` | Schema for Proof Protocol claims, including verdict values and **INVALID**. | Schema validation is format validation, not proof validation. |
| `termsOfUse` | MAY express ProofStamp™ mark-use conditions. | Does not grant trademark rights beyond those Nebulonium, Inc. grants separately. |
| `proof` and securing mechanisms (Data Integrity; JOSE/COSE) | Issuer signature over the credential. | Does not replace verification of `proofstamp.json` `authorization_signature` against the HACKERverse® public key at proofstamp.io, or PP-SPEC-003 Section 6 verification. |
| Verifiable presentation, holder, and verifier roles | A vendor or agent presenting a Proof Protocol credential to a relying party, including within a PP-A2P™ ProofPresentation (PP-SPEC-007). | A verifier evaluating an efficacy claim MUST also apply Proof Protocol verification. |

## 7. Interoperability Rules

1. The authoritative upstream source, edition/version, and applicable identifier SHOULD be recorded.
2. Upstream identifiers and terminology MUST NOT be silently redefined.
3. Proof Protocol verdicts are Proof Protocol results; they are not upstream certifications, audit opinions, legal determinations, or endorsements.
4. A control's presence, configuration, activation, detection, and efficacy are distinct facts.
5. A policy decision, alert, log entry, or control trigger does not by itself establish efficacy when the claim concerns a downstream protected outcome.
6. Target, application, tool, SIEM, vendor, service, resource, or equivalent evidence SHOULD complete the evidence round trip when required to establish the result.
7. Missing required evidence MUST yield **INVALID**, not PASS.
8. Material changes to the tested system, policy, control, environment, mapping, or corpus SHOULD trigger retesting where they can affect the result.
9. Format, envelope, transport, and registration properties MUST remain distinguishable from empirically tested efficacy assertions.
10. Expressing a Proof Protocol artifact in VC Data Model v2.0 MUST NOT drop the evidence references, verdict, version context, or **INVALID** semantics of the source artifact.
11. External-format validity under VC Data Model v2.0, such as a valid signature, receipt, credential, manifest, or message, is not by itself Proof of Efficacy.
12. The native Proof Protocol artifact remains authoritative. Any expression of it in VC Data Model v2.0 SHOULD reference it by digest (for example `verifier.root_hash` or the SHA-256 digest of `packet.json`) and by Proof Record ID where one exists.
13. A verifiable credential that carries a Proof Protocol verdict MUST include `evidence` references sufficient to locate and verify the ProofBundle™.

## 8. Framework-Agnostic Architecture

> **Threat, control, audit, regulatory, and interoperability frameworks are pluggable inputs to Proof Protocol. Proof Protocol is framework-agnostic.**

External frameworks can identify **what to test or examine**, or how a result is carried. Proof Protocol independently establishes **whether the control worked and what evidence proves that result**.

No external framework is required for Proof Protocol to operate. An implementation MAY use the W3C Verifiable Credentials Data Model v2.0, another recognized framework or standard, a proprietary threat model, several frameworks simultaneously, or no external framework when the test condition is otherwise sufficiently defined.

Adding, replacing, muting, or removing a framework mapping does not alter the Proof Protocol architecture, evidence model, Proof of Efficacy determination, ProofBundle™, ProofStamp™, ProofRegister™, or independent corroboration requirements.

A framework mapping therefore establishes **interoperability**, not architectural dependency.

## 9. Relationship to Proof Protocol

This mapping is part of the Proof Protocol specification family maintained by Nebulonium, Inc.

The relationship is intentionally asymmetric:

> **The W3C Verifiable Credentials Data Model v2.0 supplies a standard way to issue, hold, and present secured claims. Proof Protocol supplies the evidence model, and the verdicts and ProofStamp™ those claims may carry, for determining whether a selected control performed as claimed.**

No affiliation, endorsement, certification, or sponsorship by the World Wide Web Consortium (W3C) or the W3C Verifiable Credentials Working Group is implied.

## 10. Source Framework, Attribution, and License

The Verifiable Credentials Data Model v2.0 is external work of the World Wide Web Consortium, developed by the W3C Verifiable Credentials Working Group. Its status is W3C Recommendation, 15 May 2025.

The Recommendation states that W3C "liability, trademark and permissive document license rules apply", and the permissive document license link resolves to the W3C *Software and Document license - 2023 version* (https://www.w3.org/copyright/software-license-2023/). That license permits copying, modifying, and distributing the work provided copies include the license NOTICE, any pre-existing notices, and a statement that the work "includes material copied from or derived from" the identified W3C document. The separate W3C Document License (2023) (https://www.w3.org/copyright/document-license-2023/) applies to W3C documents published under it and prohibits publication of derivative works for use as a technical specification; this mapping derives no text from the Recommendation under either license.

This Proof Protocol mapping is independently authored and licensed under **CC BY 4.0**. It references VC Data Model property names, type names, context URLs, and role names for interoperability and does not relicense W3C material.

Where upstream material is copyrighted, licensed with conditions, or access-controlled, contributors MUST NOT copy upstream normative text into this repository merely to make the mapping self-contained. Reference identifiers and independently authored descriptions should be used.

### Required attribution

The Recommendation carries the following copyright notice, verbatim:

> Copyright © 2025 World Wide Web Consortium. W3C® liability, trademark and permissive document license rules apply.

Source: https://www.w3.org/TR/2025/REC-vc-data-model-2.0-20250515/. Because this mapping copies and derives no W3C text, the notice-of-changes statement required by the W3C Software and Document license is not triggered. Recommended attribution line used by this repository:

> This specification references Verifiable Credentials Data Model v2.0, W3C Recommendation, 15 May 2025, https://www.w3.org/TR/2025/REC-vc-data-model-2.0-20250515/. Copyright © 2025 World Wide Web Consortium. W3C® is a trademark of the World Wide Web Consortium.

**What this specification reproduces:** property names, type names, the base context URL, the media type, role names, and short factual statements of status and date. It does **not** reproduce Recommendation prose, examples, vocabulary definitions, JSON-LD contexts, or schemas. If W3C material is later copied or derived into this repository, the full W3C Software and Document license NOTICE and the statement "This software or document includes material copied from or derived from Verifiable Credentials Data Model v2.0, https://www.w3.org/TR/2025/REC-vc-data-model-2.0-20250515/" MUST be added.

## 11. Versioning

This mapping is versioned independently of the W3C Verifiable Credentials Data Model. A new Recommendation, an amended Recommendation, or a change to the referenced securing specifications SHOULD trigger a mapping review and, where necessary, a new version identifying the upstream version mapped.

---

*Proof Protocol · proofprotocol.io · CC BY 4.0*
