# PLAN — easy-read-plus

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: J. Carter (acting maintainer) · Lane: donated

> **Positioning:** Essential information, made truly readable — for the people most often left
> out of it. `easy-read-plus` adapts already-trustworthy public information into two
> evidence-based cognitive-accessibility formats — **Easy Read** (plain language + supporting
> symbols, for people with intellectual/learning disabilities) and **dyslexia-friendly**
> (evidence-based typography and layout) — without ever changing what the information *means*.

> **What this project is — and is not.** It is **not** a plain-language rewriting service, **not**
> an original-authoring shop, and **not** a "make it look dyslexia-friendly" font swap. It is a
> disciplined **adaptation pipeline** with three non-negotiable gates: (1) **meaning fidelity** —
> no safety-critical caveat is ever dropped; (2) **co-production** — Easy Read is validated *with*
> people with learning disabilities, never merely *for* them; (3) **license discipline on every
> layer** — source text, symbols, **and** fonts must each be openly licensed and attributed.

---

## Executive summary

`easy-read-plus` is an Elyos good-deed project that produces **cognitively accessible versions of
essential public information**. Roughly 1-in-7 people have a print/cognitive-access need —
intellectual or learning disability, dyslexia, low literacy, acquired cognitive impairment, or
limited fluency — yet the public information that most affects their lives (how a service works,
what a right is, how to stay safe, what a notice means) is overwhelmingly written for confident,
fluent readers. This project closes that gap by producing two complementary, standards-anchored
outputs from the **same vetted source**: an **Easy Read** version and a **dyslexia-friendly**
version.

The work runs in the **donated lane**: a human runs their own coding agent interactively to draft
adaptations and supporting artifacts, then opens PRs; the Elyos CLI only prepares workspaces and
opens PRs. It **never** runs headless and never invokes a coding agent itself.

The project is **medium risk tier**: cognitive-accessibility adaptation requires real domain
competence, and *simplification is where meaning gets lost*. Three things make this hard and are
therefore designed in from M0, not bolted on:

1. **Meaning fidelity.** Easy Read is a translation problem in disguise — simplifying a sentence
   can silently drop a condition, a negation, or a "only if…" that changes the answer. We treat
   fidelity exactly as `vital-info-translations` treats dosage/negation accuracy: a hard,
   checklisted, human-verified gate, with explicit agent uncertainty flags that **block** sign-off.
2. **Co-production.** The recognized standard for Easy Read (Inclusion Europe's *Information for
   All*; UK Accessible Information practice) requires that materials are **made and tested with
   people with learning disabilities**. We make reader-validation a mandatory gate — and we pay
   validators fairly, record informed consent, and commit **no personal data**.
3. **Triple-layer licensing.** Unlike a text-only project, every Easy Read document mixes three
   licensed asset classes — **source content**, **symbols/pictograms**, and **fonts**. Each has
   different, sometimes incompatible terms (e.g. ARASAAC symbols are **CC BY-NC-SA** — non-
   commercial and share-alike; Photosymbols is **proprietary**). The output license must be
   compatible with the *most restrictive* asset it contains. This is encoded as checkable data.

Honest status note: the program is real but **no specific partner organization or named
requestor is yet secured**. Until one is, `verifiedNeed` is recorded as `false` on tasks whose
value depends on a named beneficiary, and the project's Definition of Shipped (adoption by a real
audience) cannot be fully met. Securing a first partner is the top open dependency (see Open
questions and Roadmap M2). M0/M1 are deliberately partner-independent so that foundations and a
proven pipeline exist *before* a partner is found.

**Locked decisions (v0.1):**
- **Lane:** donated. No funded/API-key execution in v0.1.
- **Two outputs, one source:** every adaptation produces an Easy Read version *and* a
  dyslexia-friendly version from one provenance-tracked source.
- **Standards anchors:** Inclusion Europe *Information for All*, ISO 24495-1:2023 (Plain
  language), W3C/WAI **COGA** *Making Content Usable*, WCAG 2.2 (for output accessibility), and
  the **British Dyslexia Association (BDA) Dyslexia Style Guide** for typography.
- **Open assets only:** symbols from **ARASAAC / Mulberry / open Global Symbols sets**; fonts from
  **Atkinson Hyperlegible, OpenDyslexic, Lexend** (+ standard sans-serif per BDA). **Photosymbols,
  CHANGE picture bank, and other proprietary symbol/photo libraries are excluded** unless a
  written license is obtained.
- **Stack:** TypeScript + ESM + pnpm for tooling; **Markdown** as canonical content, exported to
  **accessible (tagged) HTML and PDF/UA**; **YAML** for metadata; JSON Schema validation in
  `packages/schema`.

## Problem & beneficiaries

**Who is helped.** People with **cognitive and print-access needs** and the practitioners who
support them:

- **People with intellectual / learning disabilities** — the primary Easy Read audience. Easy
  Read (short sentences, one idea per line, common words, a supporting image per idea) is the
  established format their advocacy organizations use and expect.
- **People with dyslexia and other reading disabilities** — served by the dyslexia-friendly output
  (typography, spacing, line length, contrast, reader font choice) rather than symbol support.
- **Adjacent beneficiaries who gain "for free":** people with low literacy, older adults with mild
  cognitive decline, people with limited language fluency, and anyone reading under stress
  (a frightening letter, an emergency). Accessible information is rarely *only* used by its target
  audience.
- **Support workers, carers, advocates, librarians, and frontline services** who need a version of
  essential information they can actually hand to the people they serve.

The ultimate beneficiary is a **person making a real decision from public information they could
not previously read** — understanding a benefit notice, a clinic appointment letter, a tenancy
right, a flood-safety leaflet — and getting it *right* because the meaning survived the
simplification.

**The need.** Cognitive accessibility is the most under-served accessibility domain. Most public
bodies publish nothing in Easy Read; what exists is often (a) out of date, (b) made *without*
people with learning disabilities, (c) inaccessible as a *file* (image-only scanned PDFs that
defeat screen readers), or (d) of unverifiable provenance. Generic LLM "simplification" is unsafe
here for the same reason generic MT is unsafe for medical text: it confidently produces fluent
output that has quietly changed the meaning, with no provenance, no symbol licensing, and no
validation by the people who must use it.

**Verified need: TO BE SECURED.** The *category* need is well-evidenced (disability-rights law and
accessible-information standards in many jurisdictions require it; advocacy orgs consistently
report scarcity). But a **specific requesting organization, target audience, and priority
document list are not yet secured.** Tasks are written so the program can build reusable
foundations (style specs, allow-lists, validation protocol, templates) and prove the pipeline on
a public document *without* a partner, while flagging that **delivery/adoption tasks are blocked**
until a partner and audience are confirmed.

**Partner / requestor: TO BE SECURED.** Candidate partner types: learning-disability advocacy and
self-advocacy organizations (e.g. Mencap, People First / self-advocacy groups, Inclusion Europe
members), dyslexia associations (e.g. the BDA), public libraries, local councils' accessible-
information teams, NHS/health-body accessible-information leads, and disability-rights NGOs. None
is committed as of this draft.

## Goals and non-goals

**Goals**
- Stand up a **repeatable, provenance-complete pipeline** that turns one openly-licensed source
  document into a reader-validated **Easy Read** version *and* a **dyslexia-friendly** version.
- Maintain three license-as-data allow-lists — **sources**, **symbols/pictograms**, and **fonts** —
  each recording exact terms, attribution, and compatibility, so the output license can be derived
  and checked automatically.
- Codify two evidence-based, standards-anchored **style specifications** (Easy Read; dyslexia-
  friendly) that a drafting agent and a human reviewer can both apply and audit.
- Make **co-production / reader-validation with people with learning disabilities** a mandatory,
  fairly-paid, consent-recorded gate for every Easy Read deliverable.
- Guarantee **meaning fidelity** — no safety-critical caveat, condition, or negation is lost in
  simplification — via a hard, human-verified gate.
- Ensure the **output files are themselves accessible** (WCAG 2.2 / PDF-UA: real text, structure,
  alt text, reading order), not just "simpler."
- Deliver adaptation sets that a partner organization or audience **actually adopts and uses**.

**Non-goals (constraints that define the project)**
- **Not original authoring.** We adapt **already-vetted, authoritative** information; we do not
  invent new facts, advice, or guidance.
- **Not advice.** We never turn essential information into personalized medical, legal, financial,
  benefits, or safety **advice**. Adapted high-stakes content always states that the original is
  authoritative and that this is information, not advice (see risk gates).
- **Not a font swap.** "Dyslexia-friendly" is the full BDA-aligned typographic + layout + plain-
  structure treatment, with **reader choice of font** — not a claim that any single font "fixes"
  dyslexia (the evidence for special dyslexia fonts is mixed; we say so honestly).
- **Not "for, without".** We will **not** publish an Easy Read document that has not been validated
  *with* people with learning disabilities. Easy Read made without its audience is out of scope.
- **Not a relicensing laundromat.** We never strip or "upgrade" a source/symbol/font license. The
  output inherits the most restrictive applicable terms.
- **Not proprietary-asset use.** No Photosymbols, no CHANGE picture bank, no stock-photo libraries,
  no commercial fonts — unless a written license is on file.
- **Not a hosting platform / portal / on-demand simplifier.** No public website that simplifies
  arbitrary user-submitted text.
- **Not machine-translation across languages** (that is `vital-info-translations` /
  `health-info-translations`); cross-language Easy Read is explicitly future/backlog and must
  combine *both* projects' gates.
- **Not partisan.** Civic/government content is adapted neutrally; we do not editorialize.

## Success metrics (outcomes)

Outcome-based and beneficiary-centric. Baselines are ~0 (new project). **Outcome** targets are for
the first ~6 months after a partner is secured; **interim foundation metrics** are tracked from
M0/M1 so progress is visible *before* a partner exists. We explicitly **do not** count "PRs
merged," "documents drafted," or "words simplified" as success.

**Outcome metrics (post-partner)**

| Metric | Baseline | Target | How measured |
|---|---|---|---|
| Reader-validated adaptation sets **adopted by a partner/audience** for real use | 0 | ≥ 3 documents (each as Easy Read + dyslexia-friendly) | Partner written confirmation in PR/receipt |
| Distinct **information domains** covered end-to-end (source → validated → delivered) | 0 | ≥ 2 | Project registry |
| **Reader comprehension** on adapted vs. original (small structured check with target readers) | n/a | Adapted ≥ original, **no comprehension regressions**, on ≥ 1 set (**effective from M2**) | Validation comprehension log |
| Easy Read sets **passing reader-validation on first or second round** | n/a | ≥ 90% (**effective from M1**, reported only once denominator ≥ 10) | Validation outcomes log |
| **Meaning-fidelity defects** found *after* delivery (dropped caveat/condition/negation, changed meaning) | n/a | 0 | Post-delivery defect log |
| **License/attribution compliance** of delivered artifacts (source + symbol + font attribution + compatible output license) | n/a | 100% (hard gate; **automated from M1**, manual + structural in M0) | License-check task / CI |
| **Output accessibility** conformance (real text, headings, alt text, reading order; WCAG 2.2 AA / PDF-UA basics) | n/a | 100% of delivered files | Accessibility checklist / automated check |
| Partner/reader-reported **usefulness** (qualitative) | n/a | Positive from ≥ 1 partner and ≥ 3 readers | Feedback log |

**Interim foundation metrics (M0/M1, partner-independent)**

| Metric | Baseline | Target | How measured |
|---|---|---|---|
| Sources **verified** + recorded on the source allow-list | 0 | ≥ 3 by end of M0 | Allow-list entries with `verifiedBy`/`verifiedDate` |
| Symbol sets + fonts **verified + license-recorded** on the asset allow-list | 0 | ≥ 2 symbol sets + ≥ 3 fonts by end of M0 | Asset allow-list entries |
| Style specs published (Easy Read; dyslexia-friendly) | 0 | 2 by end of M0 | Merged spec docs |
| Reader-validators **recruited + consented** (fairly paid) | 0 | ≥ 3 by end of M1 | Validator roster (no PII committed) |
| **Cycle time** per document (draft → validation → license-clear) | n/a | Tracked; trend down | Timestamps on PR/validation artifacts |

**Denominator & sample rule.** The denominator for the "≥ 90% pass" rate is **every Easy Read
deliverable that reaches reader-validation** (counted once per deliverable); a deliverable
"passes" if it earns validation sign-off on its **first or second** round. Reported as a
percentage **only once the denominator is ≥ 10**; below that, the raw count (e.g. "7/8 passed").

## Scope

**Definition — "essential public information" (objective).** A candidate document qualifies if it
is (a) **public-interest information** about a right, service, benefit, safety matter, civic
process, or how-to that materially affects people's lives; (b) from an **authoritative source**
(government, public health body, recognized NGO, or similarly accountable publisher); and (c)
available under an **open/PD/CC/OGL-compatible license** that permits derivative works (see Data &
licensing). **Reader-validator availability is a hard precondition, not a qualifier** — we do not
schedule an Easy Read document we cannot validate with the audience.

**Risk-domain classification (drives the review gate).**
- **Standard (medium):** general civic/service information with no high-stakes decision attached
  (how a library works, what a public notice means, how to register for a service).
- **High-stakes (elevated to high risk):** content where a misread could cause harm — **medical,
  legal, safety, benefits-eligibility, financial**. These require, *in addition to* reader-
  validation, **credentialed-expert sign-off** and verbatim "this is information, not advice; the
  original is authoritative" framing. The **M0 pilot deliberately uses a standard (non-high-stakes)
  document** to prove the pipeline before we take on high-stakes domains.

**In scope**
- **Source allow-list** (open/PD/CC/OGL sources permitting derivatives), with provenance + license
  snapshot/hash.
- **Asset allow-list** for symbols/pictograms and fonts, with per-asset license terms,
  attribution, NC/SA flags, and output-license-compatibility notes.
- Two **style specifications** (Easy Read; dyslexia-friendly), each citing its standards basis.
- **Adaptation** of specific documents into Easy Read + dyslexia-friendly outputs, decomposed per
  *document* (each producing both formats).
- **Reader-validation (co-production)** protocol, checklist, consent + fair-pay practice, and
  recorded validation outcomes.
- **Meaning-fidelity** checklist and agent uncertainty-flag mechanism.
- **Output-accessibility** checklist (WCAG 2.2 / PDF-UA basics) and reusable accessible templates.
- **License-verification** step per source, per asset, and per deliverable.
- Delivery packaging: source ref, both outputs, symbol/font manifest, provenance, attribution,
  validation record, accessibility record.

**Out of scope**
- Adapting sources **not** on the allow-list, or whose license forbids/does not clearly permit
  derivatives.
- **Authoring new advice** or altering the meaning of source information.
- **Proprietary symbols/photos/fonts** without a written license.
- **High-stakes content without credentialed-expert sign-off** (it is gated, not silently allowed).
- **Cross-language** Easy Read / translation (needs `vital-info-translations` gates too) — backlog.
- **Image-only / scanned-PDF output**, audio/video, AAC board generation, or full DTP/print
  production beyond accessible HTML + PDF/UA unless a partner requires it (then scoped as future).
- **Hosting, a portal, or an on-demand "simplify any text" service.**
- Documents for which **no reader-validator** for the audience can be sourced.

## Solution approach & architecture

This is primarily a **content/data pipeline** project (deliverables are adapted documents + data
artifacts) with light tooling. It rides existing Elyos donated-lane mechanics (CLI prepares
workspace → human runs agent → PR opened → human/expert review gates "done").

**Pipeline (per document → two outputs)**
1. **Select & verify source** — confirm the document is on the source allow-list; re-verify the
   source's current license and that **derivatives are permitted**; record provenance (URL,
   version/date, retrieval date, license snapshot + hash).
2. **Select assets** — choose symbols (Easy Read) and fonts from the **asset allow-list**; record
   exactly which assets are used and their licenses, so output-license compatibility is computable.
3. **Draft Easy Read** — agent applies the Easy Read style spec (short sentences, one idea per
   line, common words, a supporting symbol per idea, glossary for unavoidable hard words),
   preserving every condition/negation/caveat per the meaning-fidelity checklist.
4. **Draft dyslexia-friendly** — agent applies the dyslexia-friendly spec (BDA typography/layout,
   plain structure, reader font choice) to the *same* meaning-checked content.
5. **Agent self-check** — agent runs the meaning-fidelity + style checklists and **emits explicit
   `UNCERTAIN:` flags** for anything it is unsure changed meaning; flags are copied into the
   validation record and **block** sign-off until resolved.
6. **Meaning-fidelity review** — a human reviewer verifies nothing material was lost or changed
   against the source (mandatory; second reviewer + credentialed expert for high-stakes).
7. **Reader-validation (co-production)** — people with learning disabilities review the Easy Read
   version for comprehension/usability; outcomes + changes recorded (consent + fair pay; no PII).
8. **License & attribution check** — source + every symbol + every font attributed; output license
   compatible with the most restrictive asset; provenance complete.
9. **Accessibility check** — output files have real text, heading structure, alt text, correct
   reading order; pass WCAG 2.2 AA basics / PDF-UA.
10. **Package & deliver** — assemble the bundle, hand to partner/audience; record adoption +
    feedback.

**Artifacts / data model**
- `sources/allow-list.yaml` — `{ id, name, url, version, retrievalDate, licenseName, licenseUrl,
  derivativesAllowed, attributionTemplate, snapshotHash, snapshotArchiveUrl, riskDomain, notes,
  verifiedBy, verifiedDate, status }`.
- `assets/symbols.yaml` and `assets/fonts.yaml` — `{ id, name, source, licenseName, licenseUrl,
  nonCommercial, shareAlike, attributionTemplate, outputLicenseImplication, snapshotHash,
  verifiedBy, verifiedDate, status }`.
- `adaptations/<doc>/` — `source.md` (or reference), `easy-read.md`, `dyslexia-friendly.md`,
  exported `easy-read.html`/`.pdf` + `dyslexia-friendly.html`/`.pdf`, `assets-manifest.yaml`
  (every symbol/font used), `provenance.yaml`, `attribution.txt`, `validation.yaml` (reader-
  validation outcomes + agent flags + reviewer sign-off), `accessibility.yaml` (output checks),
  `glossary.yaml` (hard words explained).
- `specs/easy-read-style.md`, `specs/dyslexia-friendly-style.md`.
- `templates/easy-read-template.*`, `templates/dyslexia-friendly.css`,
  `templates/reader-validation-checklist.md`, `templates/meaning-fidelity-checklist.md`,
  `templates/accessibility-checklist.md`.

**Formats.** UTF-8 throughout; Markdown as canonical content; exported to **tagged/accessible
HTML and PDF/UA**; YAML for metadata. Symbols referenced by stable id from the asset allow-list.

**Content schemas & CI validation.** The Task JSON schema lives in
`packages/schema/src/schemas.ts` (AJV / JSON Schema **draft-07**). The project's content/data
artifacts get their **own JSON Schemas in the same package** — `sourceAllowListSchema`,
`assetAllowListSchema`, `provenanceSchema`, `validationSchema`, `accessibilitySchema` — compiled
and exposed via `validate.ts` exactly like `taskSchema`/`registrySchema` (AJV with `ajv-formats`).
YAML artifacts are parsed to JSON and validated by a structural-check script (`tooling-001`) wired
into `pnpm test`, so **CI fails on any malformed or non-conformant content file**. The license
gate (`license-002`, M1) builds on this by cross-checking each deliverable's `assets-manifest`
against the asset allow-list and computing the **required output license** (e.g. presence of any
NC asset ⇒ output must be NC). This keeps validation **agent-neutral** and in the core schema
package, not in adapters.

**Key decisions**
- **License-as-data on three layers.** Source, symbols, and fonts are each structured, checkable
  data; the output license is *derived*, not asserted.
- **Two specs, one meaning.** Both outputs derive from a single meaning-checked content core, so
  fidelity is verified once and both formats inherit it.
- **Reader-validation is a gate, not a metric.** No Easy Read ships without it.
- **Output must be accessible as a file**, not just simpler as prose.
- **No data ingestion from people.** We pull only public, allow-listed sources; validator
  participation is consented and recorded without committing PII.

## Data, licensing & compliance

**This is the project's most important section. Be conservative; when terms are unclear, do not
adapt or do not use the asset.**

Unlike a text-only project, every Easy Read deliverable combines **three independently-licensed
asset classes**. The output license must be compatible with **all** of them — i.e. with the *most
restrictive*.

**1. Source content (allow-list).** Only sources on the allow-list may be adapted, each with terms
**verified and recorded** before use. Common cases (verify per document):
- **Government / public-sector** — varies by jurisdiction: UK **Open Government Licence (OGL)**
  (permissive, attribution), **US federal works = public domain** (but pages may embed third-party
  material), Crown copyright, or all-rights-reserved. **Each verified individually.**
- **Public health bodies (WHO/national)** — often **not** generic CC-BY (e.g. WHO is frequently
  CC BY-NC-SA 3.0 IGO with a mandatory disclaimer); derivative permission must be explicit.
- **NGOs** — terms vary; many all-rights-reserved. Verify; obtain written permission where needed.
We record per source: canonical URL, version/date, retrieval date, license name + URL, a
**snapshot of the license text (committed) with SHA-256 `snapshotHash`** and a web-archive URL
where possible, whether **derivatives are permitted**, the required attribution string, and the
**risk-domain** classification.

**2. Symbols / pictograms (asset allow-list).** This is a frequently-missed compliance trap.
- **ARASAAC** — **CC BY-NC-SA**: attribution + **non-commercial** + **share-alike**. Using ARASAAC
  symbols forces the output to be **NC + SA** (blocks downstream commercial reuse; most public-good
  distribution is fine, but this must be declared, not assumed).
- **Mulberry Symbols** — **CC BY-SA** (attribution + share-alike, **commercial allowed**) —
  generally preferred where a more reusable output license is wanted.
- **Global Symbols** — an aggregator; **license is per-set** — verify each set individually.
- **Photosymbols, CHANGE picture bank, stock-photo libraries** — **proprietary / excluded** unless
  a written license is on file.
Each symbol set is recorded with `nonCommercial`/`shareAlike` flags and its **implication for the
output license**.

**3. Fonts (asset allow-list).**
- **Atkinson Hyperlegible** (SIL **OFL**), **OpenDyslexic** (open), **Lexend** (OFL) — open,
  embeddable; record OFL attribution/reserved-name conditions.
- **Standard sans-serif** (Arial, Verdana, Tahoma, Calibri, etc.) per BDA may be *referenced* in
  CSS but **not embedded/redistributed** unless licensed; prefer OFL fonts for any embedded output.
- **Honesty note (evidence):** the BDA recommends good plain typography and **reader choice**; the
  evidence that any single "dyslexia font" measurably improves reading is **mixed**. We therefore
  offer font choice and do **not** claim a font is a cure.

**BLOCKING prerequisite for the first adaptation.** Before `adapt-001` may start,
the maintainer/license reviewer must have **confirmed in writing, recorded on the relevant
allow-list entries**: (a) the **source** permits derivative works and its attribution string; and
(b) every **symbol set and font** to be used is openly licensed, with its attribution and its
output-license implication recorded. This is a **hard gate, not an open question** — if any layer
is unconfirmed, that asset/source is swapped for a compliant one or the document is not adapted.

**Snapshot integrity & drift.** License/source snapshots are committed with SHA-256 hashes (and
web-archive URLs where possible). A **source/asset-change watcher** (minimal manual/scripted check
in M0, automated in M1) re-fetches and **flags drift** for re-verification before further use.

**Provenance model.** Every deliverable ships `provenance.yaml` (source id/version/retrieval date,
glossary version, drafting agent + human, reviewer + sign-off) and an `assets-manifest.yaml`
listing every symbol and font used. Provenance + manifest are non-optional parts of the license
gate.

**Output licensing.** Code/tooling is **MIT**. Adapted **content's license is derived** from the
most restrictive input: if any ARASAAC (NC-SA) symbol is used, the output is **CC BY-NC-SA**; if
only Mulberry (BY-SA) + OGL/PD source, the output can be **CC BY-SA**; and so on. We **never**
relicense to a more permissive license than the inputs allow. Where a source forbids derivatives,
we obtain permission or do not adapt it.

**Privacy / PII & co-production ethics.** Sources are public; we ingest **no personal data**.
Reader-validators (people with learning disabilities) and reviewers participate under **informed
consent** appropriate to the audience (accessible consent), are **fairly paid** for their time
(co-production is labor, not extraction), and **no personal data, names, or recordings are
committed** to the repo — only de-identified validation outcomes and counts. Partner/validator
contact details are handled out-of-band and never committed.

**Attribution requirements.** Every deliverable includes: the source attribution string; symbol-
set attribution (e.g. "Symbols © ARASAAC, CC BY-NC-SA"); font attribution/OFL notice; and, for
high-stakes content, the verbatim "information, not advice; original is authoritative" statement.

## Quality, review & risk gates

**Risk tier: medium** (with **per-document elevation to high** for high-stakes domains). Medium =
needs domain accuracy and reviewers with the relevant skill — here, **cognitive-accessibility /
Easy Read competence** plus **reader-validators from the audience**. High-stakes domains
(medical/legal/safety/benefits/financial) additionally require **credentialed-expert sign-off**.

**Required review before a deed is "done"**
1. **Agent self-check** against the meaning-fidelity + style checklists, including emitting
   `UNCERTAIN:` flags (first pass; not sufficient alone).
2. **Meaning-fidelity review** by a competent human against the source (nothing material lost or
   changed). **Mandatory.**
3. **Reader-validation (co-production)** of the Easy Read version with people with learning
   disabilities; outcomes + resulting changes recorded. **Mandatory for Easy Read.**
4. **License & attribution verification** (source + every symbol + every font; output license
   compatible with most restrictive asset; provenance + manifest complete).
5. **Output-accessibility verification** (real text, structure, alt text, reading order; WCAG 2.2
   AA basics / PDF-UA).
6. **CI green** for tooling and structural checks on metadata.
7. **Maintainer approval** of the PR.
8. **Credentialed-expert sign-off** — **required** for any high-stakes-domain document, recorded
   in the PR, with the verbatim "not advice / original authoritative" framing present.

**Reviewer independence & two-reviewer rule.** The human who ran the drafting agent may **not** be
the sole meaning-fidelity reviewer for that deliverable; each reviewer records a conflict-of-
interest declaration. A **second independent reviewer is mandatory** for high-stakes documents (in
addition to the credentialed expert). Reader-validation must involve **people from the actual
audience**, not the drafting team standing in for them.

**Agent uncertainty self-check (operationalized).** The drafting agent emits, in a defined format,
one entry per flag: `UNCERTAIN: <location> | <type: dropped-caveat|negation|condition|term|
ambiguous-source|symbol-fit> | <note>`. Flags are copied into `validation.yaml` as `agentFlags`.
**No sign-off may be recorded while any flag is unresolved** — each must be `resolved` (with the
reviewer's adjudication) or `accepted-as-is` with reasoning. Unresolved flags **block** "done."

**Disagreement / conflict resolution.** If meaning-fidelity reviewers (or experts) disagree, the
deliverable cannot be signed off until resolved: (1) reconcile against the source, recording
rationale; (2) escalate unresolved safety-relevant disagreements to a third reviewer/expert or the
maintainer, decision recorded; (3) **when in doubt, the more conservative reading wins** and the
disputed passage is held back rather than shipped. Recorded in `validation.yaml`.

**Definition of Shipped (project-specific).** An adaptation set is *shipped* only when: acceptance
criteria met **and** meaning-fidelity review passed **and** Easy Read reader-validated **and**
(for high-stakes) credentialed-expert sign-off recorded **and** license/attribution/output-license
verified across all three asset layers **and** provenance + manifest complete **and** output files
pass the accessibility check **and** CI green **and** the set is **delivered to and adopted by the
requesting partner/audience** for real use. Merged-but-not-adopted is **not** shipped.

## Roadmap & milestones

**M0 — Foundation & cold-start (no partner required).**
Goal: stand up the standards/license/validation machinery and prove the pipeline on one **standard
(non-high-stakes)** document, end-to-end except final adoption.
Exit criteria: source allow-list with ≥ 3 verified sources (hash/archived snapshots + minimal
re-fetch check); **asset allow-list with ≥ 2 open symbol sets + ≥ 3 open fonts**, each with NC/SA
flags and output-license implication; **Easy Read** and **dyslexia-friendly** style specs merged;
**reader-validation protocol + meaning-fidelity + accessibility checklists** merged; the first
adaptation's **BLOCKING license prerequisite** confirmed across all three layers (`license-000`)
before drafting; **one standard document adapted into Easy Read + dyslexia-friendly, meaning-
fidelity reviewed, reader-validated, license-clear, and output-accessibility verified** (delivery
to a partner deferred to M2). Content JSON schemas + **minimal automated structural check** and a
**minimal source/asset-change re-fetch check** green in CI. `verifiedNeed` honestly `false` where
no partner. The "100% / ≥ 90%" metrics are **effective from M1** (M0 verifies the first deliverable
manually + structural check).

**M1 — Repeatability & validator/reviewer network.**
Goal: make the pipeline repeatable and recruit/qualify the people-gate.
Exit criteria: documented validator/reviewer-qualification criteria + **≥ 3 recruited, consented,
fairly-paid reader-validators** and ≥ 1 cognitive-accessibility reviewer (or an advocacy-org
partner engaged); style specs + templates generalized; **readability/structural lint tooling** and
**license-check tooling** (computes required output license from the assets manifest) enforced in
CI; **automated source/asset-change watcher** operating; a **second** standard document adapted &
validated; pipeline runbook merged. **Steward named** (governance prerequisite for M2).
Dependency: validator/reviewer sourcing.

**M2 — First partner delivery (needs partner).**
Goal: deliver an adopted adaptation set.
Exit criteria: a partner/audience secured (`verifiedNeed = true`); audience + priority document
list agreed; **≥ 1 reader-validated, license-clear, accessibility-verified adaptation set
(Easy Read + dyslexia-friendly) delivered and confirmed adopted**; first **reader-comprehension
check** run. First true Definition-of-Shipped event. Dependency: M0/M1 + partner.

**M3 — Scale program.**
Goal: scale across documents/domains with sustained quality and tracked outcomes.
Exit criteria: ≥ 3 documents across ≥ 2 domains adopted (including, if a partner needs it, a
high-stakes document with credentialed-expert sign-off); validator/reviewer **rotation + fair-pay
sustainability** established; outcome tracking (comprehension, post-delivery defect log, feedback)
operating; source/asset/spec maintenance cadence in effect; named sustainability owner.

## Work breakdown

The itemized, sized backlog lives in **[TASKS.md](./TASKS.md)**, organized by the milestones above
(M0–M3) plus a Backlog/future section. Each task maps to an Elyos Task JSON (see
`packages/schema/src/schemas.ts`) with id, type, lane, risk tier, deliverable, acceptance
criteria, and license fields. M0/M1 tasks are partner-independent foundations
(`verifiedNeed: false`); M2+ tasks are gated on a secured partner and marked accordingly.

## Governance, roles & stakeholders

- **Maintainer (Owner): J. Carter (acting)** — accepts/sequences tasks, approves PRs, owns the
  three allow-lists' integrity and the license gate. Acts as interim license/compliance reviewer
  until a dedicated one is named.
- **Cognitive-accessibility reviewer(s) (meaning-fidelity + Easy Read competence): TO BE SECURED** —
  verify nothing material is lost in simplification; sign off in PRs. Rotation defined in M1/M3.
- **Reader-validators (people with learning disabilities): TO BE SECURED** — the co-production gate;
  validate Easy Read comprehension/usability. **Recruited, consented, and fairly paid** (M1).
- **Dyslexia-aware reviewer / BDA-aligned reviewer: TO BE SECURED** — checks the dyslexia-friendly
  output against the style spec.
- **License/compliance reviewer** — may be the maintainer initially; verifies source + symbol +
  font terms and per-deliverable attribution/output-license. Escalates ambiguous licenses.
- **Credentialed expert reviewers** — **required** for any high-stakes-domain document
  (clinician for medical, qualified legal reviewer for legal/rights, etc.). High-stakes content
  cannot be marked done without this.
- **Steward (last-mile owner): TBD — named by end of M1** (acting maintainer holds interim). Owns
  the partner relationship and confirms **adoption**. Naming a steward is a **prerequisite for M2**.
- **Partner / requestor: TO BE SECURED** — advocacy org / public body / library defining audience
  + document priorities and confirming adoption.

## Dependencies & integrations

- **Elyos donated lane:** `packages/cli` (workspace prep + PR), `packages/core`, `packages/schema`
  (Task JSON + new content schemas). No funded-lane / API-key execution in v0.1.
- **Open symbol sets:** ARASAAC, Mulberry, Global Symbols (per-set licenses) — read-only, verified.
- **Open fonts:** Atkinson Hyperlegible, OpenDyslexic, Lexend (OFL/open).
- **Standards references:** Inclusion Europe *Information for All*; ISO 24495-1:2023; W3C/WAI COGA
  *Making Content Usable*; WCAG 2.2 / PDF-UA; BDA Dyslexia Style Guide; plainlanguage.gov / PLAIN.
- **Public source sites:** government/OGL portals, public-health bodies, NGOs (read-only;
  per-source license verification).
- **Human network (external, not yet secured):** reader-validators (via a self-advocacy/advocacy
  org), cognitive-accessibility reviewers, dyslexia-aware reviewers, credentialed experts.
- **Partner organization** for requirements + adoption — **not yet secured.**
- **Accessibility/export tooling** (Markdown → tagged HTML / PDF-UA) and an accessibility checker.

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Simplification **drops a caveat/condition/negation** or changes meaning, causing harm | Medium | Critical | Meaning-fidelity checklist + mandatory human review against source; agent `UNCERTAIN:` flags block sign-off; conservative-reading rule; second reviewer + expert for high-stakes | Reviewers / Maintainer |
| **Symbol/font license violation** (esp. using proprietary Photosymbols, or relicensing NC/SA assets too permissively) | Medium | High | Asset allow-list with NC/SA flags; output license **derived** from most restrictive asset; license-check tooling; proprietary assets excluded | License reviewer / Maintainer |
| **Source license** forbids derivatives / unverified | Medium | High | Source allow-list; `derivativesAllowed` recorded; BLOCKING `license-000`; "if unclear, don't adapt" | License reviewer |
| **Easy Read made without the audience** (no/weak reader-validation) | High | High | Reader-validation is a **hard gate**; recruit validators (M1); won't ship un-validated Easy Read | Reviewers / Steward |
| **Co-production done extractively** (unpaid/un-consented) | Medium | High | Fair-pay + accessible informed consent required; no PII committed; ethics in validator protocol | Maintainer / Steward |
| **High-stakes content** adapted without expert review / drifts into advice | Medium | Critical | Risk-domain classification; high-stakes ⇒ high tier + credentialed-expert sign-off + "not advice" framing; M0 pilot is non-high-stakes | Maintainer / Expert |
| **Output file inaccessible** (image-only PDF defeats screen readers) | Medium | Medium | Accessibility checklist (WCAG 2.2 / PDF-UA); accessible templates; output-accessibility gate | Reviewers / Maintainer |
| **Over-claiming "dyslexia font" benefits** (misinformation) | Low | Medium | Evidence-honest spec; reader font choice; no cure claims; cite BDA | Maintainer |
| **No partner secured → nothing reaches "shipped"** | High | High | M0/M1 build partner-independent value; concrete outreach plan (Open questions); `verifiedNeed=false` until secured; **pause/decision point** if none by end of M1 | Acting maintainer → Steward |
| **Source content changes/retracted** after adaptation | Medium | Medium | Version + retrieval date in provenance; snapshot hash + change watcher; re-verify before delivery; original is authoritative | Maintainer |
| **Partisan/editorializing drift** in civic content | Low | High | Non-partisan rule; neutral adaptation; reviewer check | Reviewers |

## Security & privacy

- **Threat surface** is small: public source ingestion + open assets + text/image artifacts in a
  public repo. Main risks are **integrity** (wrong/tampered source, meaning loss), **license/
  compliance**, and **co-production ethics**, not data exfiltration.
- **No secrets** in the normal flow (donated lane uses the human's own agent; no API keys, tokens,
  or escrow). Per CLAUDE.md, never write secrets/tokens into logs, receipts, or committed files.
- **PII:** none ingested from the public; **validator/reviewer identities and any recordings are
  kept out-of-band and never committed** — only de-identified validation outcomes/counts are
  stored. Consent is recorded out-of-band.
- **Abuse/misuse prevention:** refusal guardrails apply — refuse tasks that would inject
  misinformation, simplify content into misleading advice, target/deceive people, primarily
  benefit a for-profit, or violate a source/symbol/font license or a person's privacy. Source
  integrity (correct, current, authoritative) is verified per task.
- **Supply-chain:** pin source/asset URLs + version/date; hash/archive snapshots; run the change
  watcher (M0 minimal, M1 automated).

## Sustainability & maintenance

- **After delivery,** the maintainer + steward keep the three allow-lists and the style specs
  current and re-verify sources/assets when upstream changes (e.g. a symbol set changes license, a
  source is updated). Validator/reviewer **rotation + fair-pay** (M3) avoids single-person
  dependence and keeps co-production sustainable rather than extractive.
- **Outcome tracking** continues post-delivery: a per-set defect/feedback log (meaning-fidelity
  defects target 0), reader-comprehension/usefulness feedback, and periodic partner check-ins.
  Outcomes (adoption, comprehension, defects, usefulness) — not merge counts — are the maintained
  metrics.
- **Decommissioning/withdrawal:** if a source or asset license changes to forbid reuse, affected
  deliverables are flagged and, if required, withdrawn; provenance + manifest make impact
  assessment exact.

## Open questions

1. **Partner / audience (blocks M2 & `verifiedNeed=true`).** Which advocacy org / public body /
   library is the first requestor, and what is the audience + priority document list?
   **Partner-sourcing plan (concrete):**
   - **Outreach targets (named types):** learning-disability self-advocacy / advocacy orgs (Mencap,
     People First groups, Inclusion Europe members), the BDA, public libraries, council accessible-
     information teams, NHS/health-body accessible-information leads, disability-rights NGOs.
   - **Owner:** acting maintainer runs outreach until a Steward is named (by end of M1).
   - **Timeline:** outreach in parallel with M0; ≥ 3 serious conversations by end of M1; first
     partner agreed during M2 sourcing (`partner-001`).
   - **Pause/decision point:** if no partner by end of M1 (foundations complete), the maintainer
     makes an explicit **go / pivot / hold** decision (e.g. pivot to open self-publication of
     adaptations of permissively-licensed public docs) rather than drafting more no one will adopt.
2. **Reader-validator sourcing & fair pay.** Recruit individuals via an advocacy partner, or
   contract a self-advocacy group that runs validation as a paid service? What is the pay rate and
   consent process appropriate to the audience?
3. **Output license policy.** Default to **CC BY-SA** (use Mulberry symbols) to maximize reuse, or
   accept **CC BY-NC-SA** when ARASAAC's larger symbol set is needed? Per-document or project-wide?
4. **High-stakes scope.** Do we take on medical/legal/benefits content at all in v0.1 (needs
   credentialed experts), or stay standard-tier until the expert network exists?
5. **Delivery formats.** What do partners actually need — accessible HTML, PDF/UA, print-ready,
   Word? How much DTP is in scope?
6. **Relationship to sibling projects.** Coordinate with `a11y-alttext-commons` (alt text),
   `open-pictograms`, and the translation projects for any cross-language Easy Read.
7. **Funded lane?** Proposal implies donated; is metered drafting under escrow ever wanted for
   surge demand? (Out of scope for v0.1.)

## References

- `C:\code\elyos\CLAUDE.md` — Elyos work rules, lanes, quality bar, refusal guardrails.
- `C:\code\elyos\docs\good-deed-definition.md` — good-deed criteria and risk tiers.
- `C:\code\elyos\packages\schema\src\schemas.ts` — Task JSON schema (and home for content schemas).
- `C:\code\elyos\planning\ROADMAP.md` — portfolio roadmap (this project: Track 4, medium risk).
- `C:\code\elyos\planning\projects\vital-info-translations\PLAN.md` — sibling content-pipeline plan
  whose license/fidelity/review patterns this project mirrors.
- Inclusion Europe — *Information for All: European standards for making information easy to read
  and understand.*
- ISO 24495-1:2023 — *Plain language — Part 1: Governing principles and guidelines.*
- W3C/WAI **COGA** — *Making Content Usable for People with Cognitive and Learning Disabilities.*
- WCAG 2.2 and PDF/UA (PDF accessibility) — for output-file accessibility.
- British Dyslexia Association — *Dyslexia Style Guide* (typography/layout).
- plainlanguage.gov / PLAIN — Federal Plain Language Guidelines.
- ARASAAC (CC BY-NC-SA), Mulberry Symbols (CC BY-SA), Global Symbols — symbol licensing.
- Atkinson Hyperlegible (SIL OFL), OpenDyslexic, Lexend (OFL) — font licensing.
- UK Open Government Licence (OGL); US federal public-domain notice — source licensing examples.

---

## Appendix A — Improvements applied

The following 25 specific improvements were made to the first draft of this plan and **applied** in
the sections above (not merely listed):

1. **Three-layer license model** (source + symbols + fonts) made the central compliance frame,
   instead of treating licensing as a single text-source check. (Data & licensing; Architecture)
2. **Asset allow-list elevated to a first-class artifact** (`assets/symbols.yaml`,
   `assets/fonts.yaml`) with its own M0 task (`assets-001`).
3. **Output license is *derived*, not asserted** — explicit NC/SA propagation rule (ARASAAC NC-SA
   ⇒ output NC-SA; Mulberry BY-SA ⇒ output may be BY-SA). (Data & licensing; `license-002`)
4. **Proprietary assets explicitly excluded** by name (Photosymbols, CHANGE picture bank, stock
   photos, commercial fonts) to pre-empt the most common Easy Read license violation.
5. **Co-production / reader-validation made a hard mandatory gate**, not optional user testing —
   "for, without" Easy Read declared out of scope.
6. **Co-production ethics added** — accessible informed consent, **fair pay**, and **no PII
   committed** — so validation is not extractive. (Privacy; risk row; `validators-001`)
7. **Meaning-fidelity treated as a translation-grade safety gate**, reusing the proven
   `vital-info-translations` pattern (checklist + `UNCERTAIN:` flags that block sign-off).
8. **Two distinct style specs** (Easy Read vs dyslexia-friendly) recognized as different audiences
   and standards, rather than one "accessible" spec.
9. **Evidence-honest treatment of "dyslexia fonts"** — reader choice, no cure claims, BDA-cited —
   to avoid generating misinformation (a refusal-guardrail concern).
10. **Output-file accessibility made its own gate** (WCAG 2.2 / PDF-UA): "simpler prose in an
    image-only PDF" is explicitly a failure.
11. **Risk-domain classification** that **elevates high-stakes content to high tier** with
    credentialed-expert sign-off + "not advice" framing, keeping the project honestly medium.
12. **M0 pilot deliberately scoped to a non-high-stakes document** to de-risk the cold start.
13. **BLOCKING `license-000`** prerequisite covering **all three** asset layers before first draft.
14. **Snapshot hash/archive + change watcher** for sources *and* assets (symbol/font license drift),
    M0 minimal → M1 automated.
15. **Standards explicitly anchored** (Inclusion Europe, ISO 24495-1, COGA, WCAG 2.2, BDA,
    plainlanguage.gov) so specs are auditable, not opinion.
16. **Interim foundation metrics vs. outcome metrics split**, so partner-independent progress is
    measurable before a partner exists.
17. **Denominator + minimum-sample rule** for the pass-rate metric to avoid small-sample noise.
18. **Reader-comprehension metric added** (adapted ≥ original; no regressions) — the truest test of
    "did the meaning survive and become usable."
19. **Definition of Shipped** requires reader-validation + (high-stakes) expert sign-off +
    output-accessibility + adoption — not "merged."
20. **Reviewer independence, COI, two-reviewer + conservative-reading rules** carried over and
    adapted; reader-validation must use the real audience.
21. **Pause / go-pivot-hold decision point** if no partner by end of M1, with a concrete named
    outreach plan and owner.
22. **Steward named as an explicit M2 prerequisite**; governance roles list every distinct
    review role (accessibility reviewer, reader-validator, dyslexia reviewer, license, expert).
23. **CI structural check in M0, full license-derivation enforcement automated in M1**, mirroring
    the sibling project's honest staging of the 100%/≥90% gates.
24. **Sustainability addresses fair-pay rotation** (co-production must stay sustainable) and a
    decommissioning/withdrawal path if a source/asset license changes.
25. **Content JSON schemas placed in `packages/schema`** (agent-neutral core, not adapters), and a
    **schema-valid example Task JSON** with honest `verifiedNeed=false` and MIT for metadata
    provided in TASKS.md.

---

## Review sign-off

**Reviewed against the PLAN_SPEC checklist and Elyos guardrails. Findings and resolutions:**

- **Measurable metrics:** Outcome + interim tables have baselines, targets, and measurement
  methods; pass-rate has a denominator + minimum-sample rule; comprehension metric added. ✓
- **Enforceable gates:** Meaning-fidelity, reader-validation, three-layer license, output-
  accessibility, and (high-stakes) expert sign-off are each defined with a concrete artifact and a
  CI/PR enforcement point; `UNCERTAIN:` flags block sign-off. ✓
- **Risks with owners + mitigations:** 11-row table, each with likelihood/impact/mitigation/owner,
  covering the headline harms (meaning loss, license, extractive co-production, high-stakes,
  inaccessible output, no-partner). ✓
- **License / PII / expert-review guardrails:** Triple-layer license discipline with derived output
  license and excluded proprietary assets; no PII committed + fair-pay consented co-production;
  high-stakes ⇒ credentialed-expert sign-off + "not advice." Conservative ("if unclear, don't
  use/adapt"). ✓
- **Sequencing:** M0 partner-independent foundations + non-high-stakes pilot → M1 repeatability +
  people-gate + tooling → M2 partner adoption (Definition of Shipped) → M3 scale. Dependencies and
  the no-partner pause/decision point are explicit. ✓
- **Schema-valid tasks:** TASKS.md maps every field; example Task JSON validated against the
  draft-07 schema (all required fields present, enums correct, `verifiedNeed=false`). ✓
- **Issue found & fixed during review:** the first draft implied a single project-wide output
  license; corrected to a **per-deliverable derived** license (and surfaced as Open Question 3),
  because mixing ARASAAC (NC-SA) and Mulberry (BY-SA) assets across documents yields different
  output licenses. The license-check tooling (`license-002`) computes this per deliverable.
- **Issue found & fixed during review:** added an explicit **non-partisan** rule for civic content
  (Goals/non-goals + risk table) to satisfy the civic guardrail.
- **Residual human decisions required:** secure a partner/audience (Open Q1); set fair-pay +
  consent process for validators (Q2); choose default output-license policy (Q3); decide whether
  high-stakes domains are in v0.1 scope (Q4). These are flagged, not assumed.

**Status:** Ready for maintainer review. No blocking internal inconsistencies; the binding external
gate is **partner/audience + reader-validator sourcing** before any deliverable can reach
"shipped."
