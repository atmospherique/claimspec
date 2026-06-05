# Changelog

All notable changes to ClaimSpec are documented here. Format loosely follows
[Keep a Changelog](https://keepachangelog.com/); the spec uses Semantic Versioning.

## [0.1.0-alpha.3] — 2026-06-05

Second-implementer reconciliation (Mission HUD Builder — a read-only ClaimSpec
projection over its audit-log + cards + git-as-board substrate). Builder stores
flat event types and has no native Agency axis, so it is the flat-model
implementer the alpha.2 `family` question was held for. No invariant moved.

### Changed
- **`family` is now DERIVED, non-authoritative, and not required.** Removed from
  `Event.required`; redescribed as an optional `type`-derived query hint. Two
  independent implementers (Discovery via `event_type`, Builder via the audit
  `category → family` table) both assign it at the projection, never store it.
  Resolves the alpha.2 held question. `type` is the authoritative act type.

### Open (deferred)
- **Agency enum gap (raised by Builder).** `human|ai|hybrid` has no value for a
  purely mechanical system act with no epistemic content (projected to `ai`
  today, slightly over-stating AI involvement at egress). A candidate
  `automated`/`system` value — and whether it sets `ai_involved` — is deferred
  pending a third implementer. Documented in spec.md §6.4.

## [0.1.0-alpha.2] — 2026-06-05

First implementer reconciliation (Mission HUD Discovery — see
`spec/v0/conformance/discovery-mapping.md`). The `register` invariant and the
three-axis actor model matched byte-for-byte; no invariant moved.

### Added
- **`Event.review_ref`** (optional) — the human-control / review record id (EU AI Act
  Art. 50(4)); unions into `Disclosure.sources` at egress. Adopted from the reference
  implementation, which already carried it.
- **§11 Extensions** — the `x-` extension convention (namespaced, validator-ignored,
  non-authoritative), so implementations declare domain richness without bloating core.
  `patternProperties: { "^x-": {} }` added to Claim and Event.

### Pending (held)
- **`family`** — whether it is a stored field or *derived* from `type` is held for the
  **second implementer** (Builder's lean embed) to confirm against a flat-`event_type`
  model before changing the schema. Flagged provisional in §6.2.

## [0.1.0-alpha] — 2026-06-05

Initial public draft. **Not stable** — the schema may change between alphas.

### Added
- **The core move:** everything is a claim with a `confidence` and a `register`
  (`record` | `inference`).
- **The never-1.0-inference invariant** and the **phase-transition guardrail**
  (records supersede; registers don't graduate by confidence) — the central safety
  properties.
- **Object ontology:** records (`observation`, `evidence`), the inference maturity
  ladder (`hypothesis`→…→`law`), and register-less prompts.
- **Provenance spine:** event-as-truth ledger, the three-axis Actor model
  (mechanism / agency / identity), and the two input edges (`derived_from` / `drew_on`).
- **Disclosure** (egress closure-walk): AI-involvement disjunction (sticky-upward),
  external-marks union, sources union.
- **Composition** with W3C PROV, in-toto/SLSA, OpenLineage; **compliance mapping** to
  EU AI Act Art. 12 / Art. 50.
- `spec/v0/spec.md`, `spec/v0/schema.json` (JSON Schema 2020-12), an Extended-tier
  example, dual licensing (CC-BY-4.0 docs / MIT code), and governance scaffolding.

### Provenance
- Derived from the Mission HUD Discovery claim/register + provenance-spine design,
  extracted into a vendor-neutral standard (sibling to AppSpec).
