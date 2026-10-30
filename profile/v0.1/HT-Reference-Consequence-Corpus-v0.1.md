# HOLY TRINITY — FROZEN EXPERIMENT ARTIFACT
## Reference Consequence Corpus v0.1

**Status:** FROZEN FOR DUAL-INDEPENDENT-IMPLEMENTATION VALIDATION  
**Authoritative source:** `2026-09-28_holy-trinity_profile-specification-design-01.md`  
**Corpus:** 5 canonical scenarios + 12 adversarial variants = 17 cases  
**Extraction rule:** The case definitions below are extracted from Sections 22–32 of the authoritative source without substantive modification. Do not add implementation hints or project-history assumptions.

---

## 22. Canonical scenario 1 — Notice / renewal

### Authoritative modeled inputs

- term end: 31 Dec 2027;
- authoritative normalized notice deadline: `D`;
- valid termination notice must be received by `D` through the rule-defined accepted notice universe;
- auto-renewal term: 12 months;
- next-period minimum: 12,500 units;
- next-period price: €220/unit;
- complete authoritative coverage of all accepted notice channels through `D`;
- zero qualifying valid termination notices through `D`.

The corpus MUST supply `D` as a normalized instant after authoritative contractual modeling. The profile does not decide disputed legal day-counting rules.

### Required roles

`ContractStateAnchor`, termination-option `NormativePosition`, renewal `NormativePosition`, minimum-purchase `NormativePosition`, notice/renewal `ObjectiveRule` set, coverage `EvidenceAssertion`, `ConsequenceAssertion`.

### Expected consequence chain

```text
NoValidNoticeReceivedBefore(D) = true
→ TerminationOption = unavailable
→ RenewalStatus = binding
→ NextPeriodMinimum = 12,500 units
→ NextPeriodPrice = €220/unit
→ FutureContractualExposure = €2,750,000
```

### Expected classification

`computed`, provided coverage is complete and the notice-validity predicates are already authoritatively modeled.

### Failure conditions

Incomplete coverage, unresolved notice-channel scope, disputed notice validity, conflicting contract version, or ambiguous applicability MUST prevent the computed chain at the affected step.

### Non-conformant alternatives

- deriving “no notice” from an empty local search;
- treating incomplete coverage as closure;
- applying a different contract version;
- producing €2.75m without lineage to renewal status and next-period minimum;
- calling a disputed validity judgment `computed`.

## 23. Canonical scenario 2 — Qualifying fulfillment

### Inputs

- annual minimum: 10,000 units;
- authoritative accepted delivery E17: 6,500 units;
- authoritative credit/return E31: 500 units;
- E17 qualifies toward obligation O4;
- E31 offsets 500 units of the qualification created by E17.

### Expected qualification relations

```text
Q17: E17 → O4 | +6,500 units
Q31: E31 → O4 | -500 units | offsets Q17
```

### Expected consequences

```text
QualifyingFulfillment = 6,000 units
RemainingMinimum = 4,000 units
```

The product, facility, supplier, period, acceptance, substitution, and amended-scope criteria remain rule inputs. The profile standardizes the binding of those criteria to evidence, not the business criteria themselves.

## 24. Canonical scenario 3 — Amendment applicability

### Case A — next contract year

Authoritative contract state contains an amendment changing the minimum from 10,000 to 12,500 with effective scope beginning at the next contract year.

Expected:

```text
CurrentPeriodMinimum = 10,000
NextPeriodMinimum = 12,500
```

### Case B — immediate

The same amendment has an authoritative immediate effective point `T`.

Expected:

```text
Before T: CurrentPeriodMinimum = 10,000
At/after T: rule-defined current-period consequence uses 12,500 from T
```

The profile does not decide whether a mid-period amendment is prorated or retroactive unless the authoritative modeled rule explicitly says so.

## 25. Canonical scenario 4 — Reversal

### Inputs

- E17 qualifies 500 units through Q17;
- E31 is a later authoritative credit referencing E17/Q17;
- objective rule states that the credit offsets qualifying fulfillment.

### Expected

```text
Q17 = +500
Q31 = -500 offset Q17
Net qualifying fulfillment change = 0
Residual minimum increases by 500 relative to the pre-credit state
```

### Partial reversal

If E31 offsets 200 of the 500 units, Q31 = -200 and 300 units remain qualified through Q17.

### Duplicate credit

Replay of the same E31/Q31 identifiers is idempotent and MUST NOT double-reverse.

### Wrong original event

If E31 claims to offset Q99 while the authoritative evidence references E17/Q17, the relation is invalid for Q17 and MUST NOT reduce its contribution.

## 26. Canonical scenario 5 — Computed / adjudicated boundary

### Case A — timestamp rule

```text
IF notice received before D
THEN notice timely
ELSE notice late
```

With normalized authoritative timestamps and no disputed validity predicate, the consequence is `computed`.

### Case B — equitable adjustment

```text
Supplier may receive equitable adjustment
following material input-cost changes
```

`material` and the equitable amount are interpretive unless authoritatively determined.

Expected behavior:

```text
Without determination → ADJUDICATION_REQUIRED
With D7 authoritative determination → consequence may be propagated,
                                      but classification remains adjudicated
```

This is a safe-failure test, not an attempt to automate adjudication.

## 27. Adversarial variants

The minimum useful adversarial set is twelve cases:

| ID | Variant | Expected conformant behavior | Semantic ambiguity signal |
|---|---|---|---|
| A01 | Notice exactly on deadline boundary | Apply explicit inclusivity rule; if missing, escalate | Teams choose different boundary conventions |
| A02 | Ambiguous timezone | No computed deadline result until normalized | Teams silently assume local zones |
| A03 | Incomplete coverage | Do not infer non-event | One team treats zero rows as no notice |
| A04 | Conflicting coverage assertions | Escalate unless authority precedence resolves conflict | Teams choose different closure source |
| A05 | Evidence from unauthorized source | Exclude from qualification | One team accepts unrecognized evidence |
| A06 | Conflicting authoritative contract versions | `STATE_CONFLICT` | Teams pick different versions |
| A07 | Amendment lacks explicit applicability | `APPLICABILITY_UNRESOLVED` | Teams infer immediate vs next term |
| A08 | Duplicate evidence/replay | Idempotent by assertion/relation identity | One team double counts |
| A09 | Partial reversal | Offset only specified quantity | Teams reverse entire original event |
| A10 | Reversal references wrong qualification | Reject/ignore for target position | Teams match only by amount/event type |
| A11 | Adjudicated predicate marked computed | Reject classification / escalate | One team launders human judgment into computation |
| A12 | Consequence lacks minimum lineage | Non-conformant output | Team cannot reproduce result after provider replacement |

A corpus may add late evidence and retroactive amendment cases, but the twelve above already exercise the core profile boundaries.

## 28. Expected semantic outcomes

For the canonical corpus, independent implementations must agree on the following externally observable meanings:

| Case | Expected semantic outcome |
|---|---|
| C01 Notice / renewal | termination unavailable; renewal binding; next-period minimum 12,500; future contractual exposure €2.75m |
| C02 Fulfillment | qualifying fulfillment 6,000; remaining minimum 4,000 |
| C03 Amendment | next-term amendment does not alter current period; immediate amendment changes result only from effective point |
| C04 Reversal | referenced contribution reduced by exact reversal quantity; duplicates are idempotent |
| C05 Boundary | timestamp case computed; equitable-adjustment case requires adjudication unless authoritative determination supplied |

Agreement on internal data structures is irrelevant.

## 29. Failure / escalation behavior

A conformant evaluator should use a small failure vocabulary. The exact wire labels are not normative, but the meanings are:

- `STATE_CONFLICT` — authoritative contract state cannot be uniquely resolved;
- `APPLICABILITY_UNRESOLVED` — amendment/term scope is consequence-materially ambiguous;
- `AUTHORITY_INVALID` — required assertion is not from a recognized authority;
- `COVERAGE_INCOMPLETE` — a required negative fact cannot be established;
- `QUALIFICATION_UNRESOLVED` — evidence cannot be deterministically bound to the target position;
- `INPUT_INCOMPLETE` — required deterministic input is absent;
- `ADJUDICATION_REQUIRED` — a material interpretive predicate remains;
- `LINEAGE_INSUFFICIENT` — result cannot be independently reproduced/verified.

A conformant implementation MAY use different local error codes, but conformance tests must map them to these semantic classes.

## 30. Semantic-equivalence definition

Two consequences are semantically equivalent when they agree on all consequence-material dimensions:

- the affected normative position or state target;
- the resulting state/value;
- unit/dimension;
- effective time/scope;
- computed/adjudicated classification;
- materially consumed authoritative contract state, rule version, evidence/coverage set, and adjudicated determinations.

They need not share:

- serialization;
- field names;
- database schema;
- internal object identifiers;
- rule engine;
- algorithm;
- explanation prose;
- ordering of lineage items.

Equivalent numeric values expressed in different but exactly convertible units may be treated as equivalent if the corpus defines the conversion semantics.

## 31. Minimum conformance model

Conformance has three levels:

### Level 1 — Input semantic conformance

The implementation can ingest or map the six semantic roles and required relationships from external standards or local models without losing required meaning.

### Level 2 — Consequence conformance

Given the reference corpus, the implementation produces semantically equivalent expected outcomes and the expected escalation classes.

### Level 3 — Provider-replacement conformance

A second implementation, given only durable conformant artifacts and externally resolvable authoritative references, can reproduce current consequential state without access to the first provider's private reasoning or memory.

An implementation claiming full profile conformance must satisfy all three.

## 32. Reference consequence corpus

The smallest meaningful corpus is **5 canonical cases + 12 adversarial variants = 17 cases**.

Each case should contain:

```text
Case ID
Profile version
Authoritative contract-state anchor
Applicable normalized temporal scope
Normative positions
Objective rule IDs/versions and abstract semantics
Recognized authority bindings
Authoritative evidence assertions
Coverage assertions where required
Qualification relations
Expected consequence(s)
Expected computed/adjudicated classification
Expected minimum lineage
Expected escalation class, if applicable
```

The corpus should not prescribe serialization. Each team receives the same semantic case definition and may map it into its chosen stack.

No hundreds-case suite is currently justified. The corpus should grow only when a real ambiguity is discovered.
