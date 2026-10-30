# HOLY TRINITY — FROZEN EXPERIMENT ARTIFACT
## Minimum Normative Profile — Draft 0.1

**Status:** FROZEN FOR DUAL-INDEPENDENT-IMPLEMENTATION VALIDATION  
**Authoritative source:** `2026-09-28_holy-trinity_profile-specification-design-01.md`  
**Extraction rule:** The normative text below is extracted from the authoritative source without substantive modification. Do not improve, reinterpret, or extend it during the Alpha/Beta experiment.

---

# MINIMUM NORMATIVE PROFILE — DRAFT 0.1

## 1. Status and intent

This document defines the minimum shared semantics required for independent implementations to derive reproducible objective contractual consequences from authoritative modeled terms and evidence.

It is not a serialization specification, API specification, legal-rule ontology, identity system, event standard, or adjudication system.

## 2. Conformance principle

A conformant implementation MUST satisfy:

> **Equivalent authoritative normalized inputs + equivalent objective rule meaning → semantically equivalent consequence or equivalent defined escalation.**

## 3. Required semantic roles

A conformant implementation MUST support the following semantic roles, either natively or through lossless mapping:

1. `ContractStateAnchor`
2. `NormativePosition`
3. `ObjectiveRule`
4. `EvidenceAssertion`
5. `QualificationRelation`
6. `ConsequenceAssertion`

`TemporalScope`, applicability, coverage, and lineage are required semantics but need not be separate top-level objects.

## 4. `ContractStateAnchor`

A `ContractStateAnchor` MUST identify:

- a contract;
- an authoritative state/version;
- a recognized authority for that state;
- enough version lineage to distinguish or order materially relevant states;
- the state's effective temporal scope;
- the applicable amendment/effective-state mapping required by evaluated positions.

If a material state/version conflict cannot be resolved, the implementation MUST NOT compute the affected consequence.

## 5. `NormativePosition`

A `NormativePosition` MUST have a stable identifier within the authoritative contract model and MUST be bound to the contract state in which it is valid.

It MAY carry obligation/right/option/permission/prohibition classifications from an existing legal or contract model, but this profile does not mandate a legal ontology.

## 6. `TemporalScope` and applicability

Where time can affect a consequence, the relevant scope MUST be unambiguous as to boundary, inclusivity, and time reference.

A term, amendment, rule, evidence assertion, qualification, or consequence MUST NOT be applied outside its authoritative scope.

Unresolved material applicability MUST result in escalation rather than silent inference.

## 7. `ObjectiveRule`

An `ObjectiveRule` MUST identify:

- rule ID and version;
- required inputs;
- deterministic condition meaning;
- consequence meaning;
- affected normative position(s);
- temporal/applicability scope;
- required authority classes for material inputs.

The rule language is implementation-local.

A rule is computable only where all material predicates are explicit and deterministic or supplied by authoritative determinations.

## 8. `EvidenceAssertion`

An `EvidenceAssertion` MUST identify:

- assertion ID;
- assertion meaning/type;
- recognized asserting authority;
- relevant subject/contract relation;
- occurrence/effective time or scope where material;
- durable source lineage/reference.

The profile recognizes event, adjustment, coverage, and determination assertions.

Repeated delivery of the same assertion ID MUST NOT create a second economic effect.

## 9. Coverage assertions and authoritative non-events

A coverage assertion MUST additionally identify:

- covered event class;
- covered channels/source set;
- coverage interval;
- evaluation point;
- completeness state.

An implementation MUST NOT infer a contractual non-event from absence alone.

A contractual non-event MAY be derived only when complete authoritative coverage exists for the evidence universe required by the rule and no qualifying event exists within that coverage.

## 10. `QualificationRelation`

A `QualificationRelation` MUST identify:

- relation ID;
- evidence assertion;
- target normative position;
- applicable scope;
- qualification basis/rule;
- contribution/effect value and unit where quantitative;
- offset target when reversing or reducing a prior qualification and target identity matters.

The profile defines no mandatory business verb taxonomy beyond this generalized relation.

Repeated delivery of the same qualification relation ID MUST be idempotent.

## 11. `ConsequenceAssertion`

A `ConsequenceAssertion` MUST identify:

- target normative position or contractual state;
- resulting state/value;
- unit/dimension where applicable;
- effective scope;
- classification as `computed` or `adjudicated`;
- minimum derivation lineage;
- profile version.

A computed consequence MUST NOT be emitted where a material interpretive predicate remains unresolved.

## 12. Minimum derivation lineage

Every consequence MUST contain or durably reference:

- contract ID;
- authoritative contract-state/version ID;
- target normative position ID;
- rule ID and version;
- evaluation temporal scope/point;
- materially consumed evidence IDs;
- materially consumed coverage IDs;
- materially consumed qualification IDs;
- adjudicated determination IDs, if any;
- profile version.

No vendor-private cognition may be a required lineage dependency.

## 13. Computed and adjudicated classification

`computed` means the consequence follows deterministically from explicit authoritative modeled inputs.

`adjudicated` means a material human/legal/business determination is required or materially supplies an input.

A consequence materially dependent on an adjudicated determination MUST preserve adjudicated provenance even when subsequent processing is deterministic.

## 14. Required failure behavior

A conformant implementation MUST decline computed output when any of the following is consequence-material:

- unresolved authoritative state/version conflict;
- unresolved applicability;
- missing required authority;
- incomplete evidence coverage for a required non-event;
- unresolved evidence qualification;
- missing deterministic input;
- unresolved adjudicative predicate;
- insufficient lineage for verification.

The implementation may use local error labels if the result can be mapped to the corresponding semantic failure class.

## 15. Semantic equivalence

Two outputs are semantically equivalent when they agree on:

- affected position/state;
- resulting value/state;
- unit/dimension;
- effective scope;
- computed/adjudicated classification;
- material authoritative basis.

Serialization, internal object model, field names, rule language, algorithm, and locally generated consequence IDs are not part of equivalence.

## 16. Authority

The profile assumes external identity and authorization mechanisms.

An assertion is authoritative only when its source is recognized for the contractual role it claims.

The profile MUST NOT create a global authority registry.

## 17. Provider replacement

A fully conformant implementation MUST externalize or durably reference every material input required to reproduce its consequences.

A replacement implementation MUST be able to reconstruct the covered consequential state without access to the prior provider's hidden reasoning, model internals, private memory, or undocumented inference.

## 18. Legacy contracts and AI

Profile conformance begins after authoritative modeling.

AI MAY assist extraction and modeling, but an AI-generated interpretation is not authoritative merely because it is plausible.

Hidden model reasoning MUST NOT be required to reproduce a computed consequence.

## 19. Explicit exclusions

This profile does not define accounting, treasury, budgeting, risk, payment execution, settlement rails, identity architecture, key management, wallets, blockchain, tokens, universal economic state, natural-language contract interpretation, judicial reasoning, APIs, storage, services, or deployment architecture.

## 20. Required conformance corpus

A conformant implementation MUST pass the reference corpus containing:

- notice/renewal;
- qualifying fulfillment;
- amendment applicability;
- reversal/offset;
- computed/adjudicated boundary;
- adversarial variants covering deadline boundary, timezone ambiguity, coverage incompleteness/conflict, unauthorized evidence, version conflict, unresolved amendment applicability, duplicates, partial/wrong-target reversals, adjudication laundering, and insufficient lineage.

---
