# TASKS — easy-read-plus

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-29 · Owner: J. Carter (acting maintainer) · Lane: donated

The backlog for the `easy-read-plus` good-deed project. Read alongside [PLAN.md](./PLAN.md).
Milestones (M0–M3) match the roadmap there.

## How these tasks map to Elyos

Each task below becomes an **Elyos Task JSON** validated against
`packages/schema/src/schemas.ts` (AJV / JSON Schema draft-07). Field mapping:

- **id** — stable slug id, e.g. `easy-read-plus-sources-001` (table column `ID`).
- **title** — the task title.
- **project** — always `easy-read-plus`.
- **type** — one of `code | research | writing | data | design-spec | maintenance` (table `Type`).
- **lane** — `donated` for all current tasks (no funded/API execution). Funded tasks would need
  `fundedBudgetUsd`; none here.
- **priority** — `high | medium | low`.
- **domain** — array, e.g. `["accessibility","plain-language","disability"]`.
- **riskTier** — `low | medium | high`. Adaptation/spec/validation tasks are **medium** (need
  cognitive-accessibility competence + reader-validation); pure tooling/process tasks are **low**;
  any **high-stakes-domain** adaptation is elevated to **high** (credentialed-expert sign-off).
- **urgent** — boolean (default false; no live emergency).
- **deliverable** — `pr | dataset | document | translation` (table `Deliverable`). Adapted
  documents use `document`; allow-lists/manifests use `dataset`; tooling uses `pr`. (Easy Read is an
  *adaptation*, not a cross-language `translation`.)
- **tokenEstimate** — `small | medium | large` (table `Size`).
- **status** — `open | in-progress | review | delivered | done` (all start `open`).
- **context / objective** — why + what.
- **acceptanceCriteria[]** — checkable bullets (listed below tables for key tasks).
- **resources[]** — links/paths (allow-lists, specs, source URLs, standards, templates).
- **output** — path/description of the produced artifact.
- **requestor** — partner/requestor; `TO BE SECURED` until a partner is confirmed.
- **verifiedNeed** — boolean; **`false`** wherever value depends on an unsecured partner (M0/M1
  foundations); flips to `true` only when a partner is confirmed (M2).
- **outputLicense** — **MIT** for code/tooling and for project metadata (allow-lists, schemas);
  for **adapted content** the license is **derived** from the most restrictive asset used. **Default
  policy (v0.2): target `CC BY-SA 4.0` via Mulberry symbols** so outputs stay reusable by any
  downstream public body; **`CC BY-NC-SA 4.0` only when ARASAAC** is used for concept coverage (the
  NC clause is ambiguous for public-sector reuse, so ARASAAC is opt-in).

---

## Milestone M0 — Foundation & cold-start (no partner required)

Goal: stand up standards/license/validation machinery and prove the pipeline on one **standard
(non-high-stakes)** document, end-to-end except final adoption.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| easy-read-plus-sources-001 | Build source allow-list (open/PD/CC/OGL) with license terms + snapshot hash/archive | data | small | low | dataset | — | Maintainer / License reviewer |
| easy-read-plus-assets-001 | Build symbol + font asset allow-list with per-asset licenses (NC/SA flags, output-license implication) | data | small | low | dataset | — | License reviewer / Maintainer |
| easy-read-plus-spec-001 | Easy Read style specification (Inclusion Europe / ISO 24495-1 / COGA; reconcile with easyreadstandard.org + record Inclusion Europe logo eligibility; state readability formulas are advisory only) | design-spec | medium | low | document | — | Cognitive-accessibility reviewer |
| easy-read-plus-spec-002 | Dyslexia-friendly typography + layout specification (BDA, evidence-based) | design-spec | medium | low | document | — | Dyslexia-aware reviewer |
| easy-read-plus-validate-001 | Reader-validation (co-production) protocol + checklist + consent/fair-pay ethics | writing | small | medium | document | — | Cognitive-accessibility reviewer |
| easy-read-plus-fidelity-001 | Meaning-fidelity checklist + agent `UNCERTAIN:` flag format + back-translation / round-trip comprehension check | writing | small | low | document | — | Maintainer |
| easy-read-plus-a11y-001 | Accessible-output checklist (WCAG 2.2 AA / PDF-UA basics) + alt-text policy (no sentence duplication; decorative `alt=""`; coordinate with a11y-alttext-commons) | writing | small | low | document | — | Maintainer |
| easy-read-plus-license-000 | **BLOCKING:** confirm derivative permission + attribution for first source AND every symbol/font used | research | small | low | document | sources-001, assets-001 | License reviewer / Maintainer |
| easy-read-plus-adapt-001 | Adapt one **standard** document into Easy Read + dyslexia-friendly (reader-validated) | writing | medium | medium | document | spec-001, spec-002, validate-001, fidelity-001, a11y-001, **license-000** | Cognitive-accessibility + dyslexia-aware reviewers + reader-validators |
| easy-read-plus-tooling-001 | Content JSON schemas + CI structural check + minimal source/asset-change re-fetch | code | small | low | pr | sources-001, assets-001 | Maintainer |

**Acceptance criteria — key M0 tasks**

`sources-001` (source allow-list)
- `sources/allow-list.yaml` lists ≥ 3 sources, each from an authoritative publisher, with terms
  permitting **derivative works**; records name, canonical URL, version/date, retrieval date,
  license name + URL, `derivativesAllowed`, required attribution string, **risk-domain**
  classification, `snapshotHash` (SHA-256 of captured license/source text), and `snapshotArchiveUrl`
  where available.
- Each entry has `verifiedBy` + `verifiedDate`; any source with unclear/incompatible terms is
  marked `excluded` with a reason. At least one entry is **standard (non-high-stakes)** for the
  pilot.
- Validates against `sourceAllowListSchema` and passes CI structural checks (`tooling-001`).

`assets-001` (symbol + font allow-list) — **critical license gate**
- `assets/symbols.yaml` lists ≥ 2 **open** symbol sets with **Mulberry (CC BY-SA) as the default**
  and **ARASAAC (CC BY-NC-SA) opt-in**, each with `licenseName`, `licenseUrl`, the **pinned exact CC
  version** (e.g. Mulberry CC BY-SA 2.0 UK), `nonCommercial`/`shareAlike` flags, attribution
  template, and `outputLicenseImplication` (Mulberry ⇒ output may be BY-SA; ARASAAC ⇒ output must be
  NC-SA, with a note that NC is ambiguous for public-sector reuse). Sclera (CC0-ish/PD) and Global
  Symbols (per-set) may be listed with per-set verification.
- **Per-pictogram provenance** is required where a set (notably **ARASAAC**) mixes third-party
  contributions — license is recorded at the symbol level in `assets-manifest.yaml`, not just per-set.
- `assets/fonts.yaml` lists ≥ 3 **open** fonts (e.g. Atkinson Hyperlegible, OpenDyslexic, Lexend)
  with OFL/open license + attribution/reserved-name notes.
- **Proprietary assets (Photosymbols, CHANGE picture bank, Widgit, PCS/Mayer-Johnson,
  SymbolStix/n2y, stock photos, commercial fonts) are recorded as `excluded` with reason** — none
  may be used.
- Each entry has `verifiedBy` + `verifiedDate`; validates against `assetAllowListSchema`.

`license-000` (BLOCKING prerequisite of `adapt-001`)
- For the **specific document + the exact symbols and fonts** chosen for `adapt-001`, confirm and
  record **in writing on the allow-list entries**: (a) source permits derivative works + its
  attribution; (b) every symbol set and font is openly licensed, with attribution and
  output-license implication recorded.
- If any layer cannot be confirmed, that source/asset is **swapped for a compliant one** (or the
  document is not adapted). `adapt-001` **must not start** until this passes.

`adapt-001` (first adaptation — standard/non-high-stakes)
- `license-000` passed across all three layers **before drafting**.
- **Upstream co-design touchpoint held** before drafting (people with intellectual disabilities
  consulted on priority/framing/known confusions), recorded in `validation.yaml`.
- The chosen **standard (non-high-stakes)** document is adapted into **both** an Easy Read version
  (`easy-read.md` + accessible HTML/PDF, one supporting symbol per idea, short sentences, glossary
  for hard words) and a **dyslexia-friendly** version (`dyslexia-friendly.md` + styled HTML/PDF per
  `spec-002`).
- **Meaning fidelity:** every condition, negation, caveat, and "only if…" in the source is
  preserved; meaning-fidelity reviewer signs off against the source **including a back-translation /
  round-trip comprehension check** (an independent restatement compared to source intent); agent
  `UNCERTAIN:` flags are all `resolved`/`accepted-as-is` (none open) in `validation.yaml`.
- **Reader-validated:** ≥ 1 reader-validation round with people with learning disabilities; outcomes
  + resulting edits recorded in `validation.yaml`; consent + fair pay handled out-of-band, **no PII
  committed**.
- Ships `provenance.yaml`, `assets-manifest.yaml` (every symbol/font used), `attribution.txt`
  (source + symbol + font attribution), and `accessibility.yaml` (real text, headings, alt text,
  reading order — passes the `a11y-001` checklist).
- **Output license is derived** from the assets used and recorded (not relicensed more permissively
  than inputs allow). Reviewer is **independent of the drafting step** (COI declared).

`validate-001` (reader-validation protocol)
- Documents how Easy Read is validated **with** people with learning disabilities: recruitment via
  an advocacy/self-advocacy route, **accessible informed consent**, **fair pay**, what validators
  check (comprehension, image fit, usability), how outcomes/changes are recorded, and the rule that
  **no Easy Read ships without validation**. Defines that **no PII** is committed.
- Covers **upstream co-production** as well as downstream: people with intellectual disabilities act
  as **consultants at the start** (co-design / priority-setting) **and quality checkers at the end**
  (per NHS England best practice), not only post-draft.
- **Defines the validation sample/method concretely** so "reader-validated" is auditable, not vibes:
  how many readers per round, recruitment route, comprehension instrument, and the **"pass"
  threshold** — and states that readability formulas (Flesch-Kincaid etc.) are **advisory only** and
  never substitute for reader-validation.
- **≥ 1 reader-validator / advocacy validation route is secured in M0** (recruitment runs in
  parallel from M0) so the M0 pilot can actually be reader-validated; the network scales to ≥ 3 in
  M1 (`validators-001`).

**M0 Definition of Done:** source allow-list (≥ 3 verified, derivative-permitting sources, snapshot
hash/archive) + asset allow-list (≥ 2 open symbol sets — **Mulberry default, ARASAAC opt-in**,
pinned CC versions, per-pictogram provenance where needed; ≥ 3 open fonts, NC/SA flags) + both style
specs (house style reconciled with easyreadstandard.org, logo eligibility recorded) + reader-
validation (sample/method defined; upstream co-design + back-translation)/meaning-fidelity/
accessibility (alt-text policy) checklists merged; **≥ 1 reader-validator / advocacy validation route
secured in M0** (parallel recruitment); **first source + assets' derivative/attribution permission
confirmed (`license-000`)** before drafting; **one standard document adapted into Easy Read +
dyslexia-friendly, meaning-fidelity reviewed (incl. back-translation), reader-validated, license-
clear, and output-accessibility verified**; content JSON schemas + minimal structural check +
minimal source/asset-change re-fetch green in CI. "100%/≥90%" metrics **effective from M1**.
Partner adoption deferred to M2 (M0 deliverables carry `verifiedNeed: false`).

---

## Milestone M1 — Repeatability & validator/reviewer network

Goal: make the pipeline repeatable and recruit/qualify the people-gate.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| easy-read-plus-validators-001 | Validator/reviewer qualification + recruitment (reader-validators + accessibility/dyslexia reviewers), ethics + fair pay | research | medium | medium | document | validate-001 | Maintainer / Steward |
| easy-read-plus-readability-001 | Readability + structural lint tooling (sentence length, word complexity, layout/alt-text checks) | code | medium | low | pr | spec-001, spec-002, tooling-001 | Maintainer |
| easy-read-plus-license-002 | License-check tooling: validate manifest vs. asset allow-list + **compute required output license** | code | medium | low | pr | assets-001, tooling-001 | License reviewer / Maintainer |
| easy-read-plus-template-001 | Reusable Easy Read template + dyslexia-friendly stylesheet (accessible HTML/PDF) | design-spec | small | low | document | spec-001, spec-002, a11y-001 | Dyslexia-aware reviewer |
| easy-read-plus-watcher-001 | Automate source/asset-change watcher (hash-diff vs. stored snapshots) | code | small | low | pr | tooling-001 | Maintainer |
| easy-read-plus-adapt-002 | Adapt a **second standard** document (different topic/format), reader-validated | writing | medium | medium | document | adapt-001, template-001 | Cognitive-accessibility + dyslexia-aware reviewers + reader-validators |
| easy-read-plus-process-001 | End-to-end pipeline runbook | writing | small | low | document | adapt-001, license-002 | Maintainer |

**Acceptance criteria — key M1 tasks**

`validators-001`
- Objective criteria for each review role: **reader-validators** (people with learning disabilities,
  from the audience, fairly paid, consented), **cognitive-accessibility/meaning-fidelity reviewer**,
  and **dyslexia-aware reviewer**. Defines **reviewer independence** (drafting human ≠ sole
  reviewer), the **second reviewer + credentialed-expert requirement for high-stakes**, the
  **disagreement/conflict-resolution** rule (reconcile → escalate → conservative reading wins,
  recorded in `validation.yaml`), and the **fair-pay + consent + no-PII** practice.
- ≥ 3 reader-validators recruited/consented and ≥ 1 accessibility reviewer engaged (or an advocacy-
  org partner engaged to run validation).

`license-002`
- Tooling reads each deliverable's `assets-manifest.yaml` + `provenance.yaml`, validates against the
  asset/source allow-lists, **computes the required output license** (any NC asset ⇒ NC; any SA
  asset ⇒ SA), and **fails CI** if the declared output license is more permissive than required, or
  if any source/symbol/font attribution is missing. This makes the "100% license/attribution" gate
  **automated from M1**.

`readability-001`
- Lints adapted content against the style specs: flags long sentences, complex/uncommon words
  (Easy Read), and layout/typography deviations (dyslexia-friendly); checks every image has alt
  text and the document has a heading structure. Output is advisory with a clear caveat that
  **metrics do not replace human meaning-fidelity review or reader-validation**.

**M1 Definition of Done:** qualification criteria published (independence, two-reviewer + expert for
high-stakes, conflict resolution, fair pay/consent/no-PII); ≥ 3 reader-validators + ≥ 1 reviewer
engaged; readability lint + **license-check tooling enforced in CI** (computes required output
license); **automated source/asset-change watcher operating**; reusable templates merged; a second
standard document adapted & validated; pipeline runbook merged. **Steward named** (M2 prerequisite).

---

## Milestone M2 — First partner delivery (needs partner)

Goal: deliver an adopted adaptation set. **All tasks gated on a secured partner**
(`verifiedNeed` flips to `true` only when the partner is confirmed).

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| easy-read-plus-partner-001 | Secure first partner/audience; agree document list + audience | research | medium | low | document | validators-001 | Steward / Maintainer |
| easy-read-plus-adapt-003 | Adapt partner-priority document set into Easy Read + dyslexia-friendly, reader-validated | writing | large | medium | document | partner-001, template-001, license-002 | Reviewers + reader-validators (+ expert if high-stakes) |
| easy-read-plus-delivery-001 | Package + deliver set; confirm adoption + run reader-comprehension check | writing | small | medium | document | adapt-003 | Steward |

**Acceptance criteria — key M2 tasks**

`partner-001`
- Outreach (owned by acting maintainer → Steward) targets named candidate types (learning-
  disability advocacy/self-advocacy orgs, BDA, libraries, council accessible-info teams, NHS/health
  accessible-info leads, disability-rights NGOs), aiming for **≥ 3 serious conversations by end of
  M1**.
- **Pause/decision point:** if no partner by end of M1, the maintainer makes an explicit
  **go / pivot / hold** decision before further adaptation work.
- A named partner/audience confirmed in writing; agreed audience + prioritized document list;
  reader-validator coverage for the audience confirmed; **any high-stakes document flagged for
  credentialed-expert sign-off**. On completion, related tasks update `requestor` +
  `verifiedNeed: true`.

`delivery-001`
- Delivered set includes Easy Read + dyslexia-friendly outputs, provenance, assets manifest,
  attribution, validation record, accessibility record; license check green (output license correct
  for assets used); **partner confirms adoption for real use in writing** (recorded in PR/receipt);
  a **reader-comprehension check** is run (adapted ≥ original, no regressions). First true
  **Definition of Shipped** event.

**M2 Definition of Done:** partner secured (`verifiedNeed=true`); ≥ 1 reader-validated, license-
clear, accessibility-verified adaptation set **delivered and confirmed adopted**; first reader-
comprehension check recorded.

---

## Milestone M3 — Scale program

Goal: scale across documents/domains with sustained quality and tracked outcomes.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| easy-read-plus-scale-001 | Adapt additional documents across ≥ 2 domains (incl. high-stakes w/ expert sign-off if needed) | writing | large | medium | document | delivery-001 | Reviewers + reader-validators (+ expert) |
| easy-read-plus-rotation-001 | Validator/reviewer rotation + fair-pay sustainability | maintenance | small | low | document | validators-001 | Maintainer / Steward |
| easy-read-plus-outcomes-001 | Outcome tracking: comprehension/usefulness + post-delivery meaning-fidelity defect log | data | small | low | dataset | delivery-001 | Steward |
| easy-read-plus-maint-001 | Source/asset/spec re-verification + license-drift maintenance cadence | maintenance | small | low | document | sources-001, assets-001, watcher-001 | Maintainer |

**Acceptance criteria — key M3 tasks**

`outcomes-001`
- A maintained log capturing, per delivered set: adoption status, **post-delivery meaning-fidelity
  defects (target 0)**, reader-comprehension results, and usefulness feedback; feeds PLAN.md success
  metrics. Stores **no PII**.

`maint-001`
- Documented cadence to re-verify source + symbol + font licenses (detect upstream/license change
  via stored snapshot hashes) and refresh the style specs; defines the **withdrawal procedure** if
  a source/asset license changes to forbid reuse (provenance + manifest scope the impact).

`scale-001`
- Any **high-stakes** document carries **credentialed-expert sign-off** and verbatim "information,
  not advice; original is authoritative" framing, in addition to reader-validation. Output license
  correctly derived per document.

**M3 Definition of Done:** ≥ 3 documents across ≥ 2 domains adopted; validator/reviewer rotation +
fair-pay operating; outcome tracking live; source/asset/spec maintenance cadence in effect; named
sustainability owner.

---

## Backlog / future

Sized but unscheduled:

- **(large) Cross-language Easy Read** — Easy Read in additional languages; **must combine both this
  project's gates and `vital-info-translations`/`health-info-translations` translation gates**
  (qualified language reviewer + this project's reader-validation). Not v0.1.
- **(medium) High-stakes domain pack** — medical/legal/benefits Easy Read, **only** once the
  credentialed-expert reviewer network exists; each item is **high** risk tier.
- **(medium) Print-ready / DTP delivery** — partner-driven layout beyond accessible HTML + PDF/UA.
- **(medium) Easy Read symbol-fit ML assist** — suggest candidate symbols from the allow-list for a
  given idea (human chooses); strictly assistive, never auto-final.
- **(small) Word/DOCX accessible export** if partners require editable formats.
- **(large, funded — needs escrow) Surge drafting under funded lane** — metered drafting against a
  hard per-task `fundedBudgetUsd`; out of scope for v0.1.

---

## Example task JSON

Schema-valid Task JSON for the first M0 task. `verifiedNeed` is **false** (no partner secured);
`outputLicense` is **MIT** because the allow-list is project metadata, not adapted source content.

```json
{
  "id": "easy-read-plus-sources-001",
  "title": "Build source allow-list (open/PD/CC/OGL) with license terms + snapshot hash/archive",
  "project": "easy-read-plus",
  "type": "data",
  "lane": "donated",
  "priority": "high",
  "domain": ["accessibility", "plain-language", "disability", "licensing"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "dataset",
  "tokenEstimate": "small",
  "status": "open",
  "context": "easy-read-plus adapts vetted, openly-licensed public information into Easy Read and dyslexia-friendly versions. Nothing may be adapted until its source is allow-listed with verified terms that permit derivative works. Source licenses vary widely (UK OGL, US federal public domain but may embed third-party material, WHO CC BY-NC-SA IGO with mandatory disclaimer, all-rights-reserved NGO content), so per-source verification is mandatory before any adaptation.",
  "objective": "Create a structured, per-source allow-list recording each approved source's license, derivative permission, required attribution, risk-domain classification, and a hashed/archived license snapshot, so the per-deliverable license gate can be checked consistently and source drift detected.",
  "acceptanceCriteria": [
    "sources/allow-list.yaml lists >= 3 authoritative sources whose terms permit derivative works, including at least one standard (non-high-stakes) source for the M0 pilot",
    "Each entry records: name, canonical URL, version/date, retrieval date, license name + URL, derivativesAllowed flag, required attribution string, risk-domain classification, snapshotHash (SHA-256), and snapshotArchiveUrl where available",
    "Each entry has verifiedBy and verifiedDate; any source with unclear or incompatible terms is marked excluded with a reason",
    "No source whose license forbids or does not clearly permit derivatives is listed as usable",
    "File validates against sourceAllowListSchema and passes CI structural checks (tooling-001)"
  ],
  "resources": [
    "C:/code/elyos/planning/projects/easy-read-plus/PLAN.md",
    "C:/code/elyos/docs/good-deed-definition.md",
    "UK Open Government Licence (OGL) terms",
    "US federal public-domain / content-use notices",
    "Inclusion Europe Information for All standards"
  ],
  "output": "sources/allow-list.yaml plus a short README documenting the allow-list schema and verification process",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "MIT"
}
```
