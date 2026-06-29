# Competitive & Improvement Analysis — `easy-read-plus`

> Scope: rigorous review of `PLAN.md` (v0.1, donated lane, medium risk) plus a web-grounded
> competitive landscape, gap analysis, differentiators, Claude-API leverage, optimizations,
> spin-offs, and open questions. Guardrails honored throughout: Easy Read standards (Inclusion
> Europe / UK practice), **co-production with people with intellectual disabilities as the
> cardinal requirement**, accuracy-when-simplifying, symbol-set licensing, and output
> accessibility.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually strong for a v0.1 draft. It correctly identifies the three hard problems —
meaning fidelity, co-production, and triple-layer licensing — and designs each as a *gate*, not a
metric. Findings below are ordered by importance.

**1.1 Easy Read standards conformance — solid, with one notable omission.**
The plan anchors to Inclusion Europe's *Information for All*, ISO 24495-1:2023, W3C/WAI COGA, WCAG
2.2/PDF-UA, and the BDA Style Guide. These are the right anchors. *Information for All* is the
recognized European standard and explicitly grants people with intellectual disabilities a *right*
to easy-to-read information ([Inclusion Europe](https://www.inclusion-europe.eu/easy-to-read-standards-guidelines/);
[PDF](https://www.inclusion-europe.eu/wp-content/uploads/2017/06/EN_Information_for_all.pdf)). ISO
24495-1 was published June 2023 and is the international plain-language standard
([ISO](https://www.iso.org/standard/78907.html)). **Omission:** the plan does not mention that
Inclusion Europe maintains an **official "European Easy-to-Read" logo/trademark** governing who may
claim conformance, nor the emerging **easyreadstandard.org** evidence-based framework now used in
the UK ([Easy Read Standard](https://easyreadstandard.org/)). The style spec (`spec-001`) should
record whether the project may display the logo (it generally requires validation by people with
intellectual disabilities, which the project does enforce) and should reconcile its house style
against easyreadstandard.org so reviewers have a single auditable basis.

**1.2 Co-production (the cardinal requirement) — correctly made a hard gate; under-specified on
method.** The plan's single best decision is declaring "**for, without**" Easy Read out of scope and
making reader-validation a blocking gate with fair pay, accessible consent, and no PII committed.
This matches NHS England guidance ("the best way to find out how to make information accessible is
to ask the person who will receive it") and the "Nothing About Us Without Us" ethos that CHANGE and
Mencap operationalize via trained checker teams
([NHS England](https://www.england.nhs.uk/learning-disabilities/about/get-involved/involving-people/why-is-it-important-to-involve-people/);
[CHANGE](https://www.changepeople.org/); [Mencap](https://www.mencap.org.uk/easy-read-library)).
**Gaps:** (a) co-production should begin *before* drafting, not only validate after — best practice
engages people with intellectual disabilities as *consultants at the start and quality checkers at
the end* ([NHS England guide](https://www.england.nhs.uk/wp-content/uploads/2018/06/LearningDisabilityAccessCommsGuidance.pdf)).
The pipeline (steps 3-7) currently inserts readers only at validation; add an upstream "co-design /
priority-setting" touch. (b) Sample size and "what good looks like" for a validation round are
undefined — how many readers, recruited how, with what comprehension instrument. (c) The plan risks
a single-person-dependency: with no partner and no validators yet secured, the *entire* cardinal
gate is unstaffed (honestly flagged, but it is the binding constraint — see §1.6).

**1.3 Accuracy preservation when simplifying — best-in-class framing.** Treating simplification as a
translation-grade safety problem (meaning-fidelity checklist + `UNCERTAIN:` flags that *block*
sign-off + conservative-reading rule + second reviewer for high-stakes) is exactly right and is
supported by the evidence that LLM document-level simplification still drops/distorts meaning and
"requires human input to ensure accuracy"
([feasibility study, PMC](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11795904/);
[LLM simplification benchmark](https://www.aimodels.fyi/papers/arxiv/redefining-simplicity-benchmarking-large-language-models-from)).
**Refinement:** add a *back-translation / round-trip comprehension* check (have a reader or
independent reviewer state, in their own words, what the Easy Read says, then compare to source
intent) — cheaper and more sensitive to silent meaning-loss than a checklist alone.

**1.4 Symbol-set licensing — the strongest section, and materially correct.** The plan's
triple-layer (source + symbols + fonts), license-*derived*-not-asserted model is excellent and rare.
Its specific claims check out:
- **ARASAAC = CC BY-NC-SA 4.0** (attribution + non-commercial + share-alike), author Sergio Palao,
  owner Government of Aragón — confirmed; using it forces NC-SA output
  ([ARASAAC via Global Symbols](https://globalsymbols.com/symbolsets/arasaac);
  [OpenSymbols](https://www.opensymbols.org/repositories/arasaac)).
- **Mulberry = CC BY-SA** (commercial allowed) — confirmed; the more reusable choice
  ([Mulberry](https://mulberrysymbols.org/); [GitHub](https://github.com/mulberrysymbols/mulberry-symbols)).
  *Minor correction:* upstream is CC BY-SA **2.0 UK** historically (now also offered as 3.0/4.0 in
  some redistributions) — `assets/symbols.yaml` should pin the exact version, since SA-compatibility
  across CC versions is non-trivial.
- **Photosymbols = proprietary subscription**, and critically: creating Easy Read for *other*
  organizations to publish requires a separate **£3,500 Extended Use** licence — confirmed; correct
  to exclude ([Photosymbols licence](https://www.photosymbols.com/pages/licence-agreement);
  [pricing](https://www.photosymbols.com/products/extended-use-licence)).
- **Widgit / PCS (Mayer-Johnson) / SymbolStix (n2y) = proprietary**, subscription/redistribution-
  licensed — correctly excluded ([Widgit licensing](https://www.widgit.com/symbol-services/licensing.htm);
  [symbol-set comparison](https://www.spectronics.com.au/article/symbol-set-comparison)).
- **CHANGE picture bank = proprietary/subscription** — correctly excluded ([CHANGE](https://www.changepeople.org/)).
**One real gap:** the plan treats "ARASAAC ⇒ output must be NC-SA" as a clean rule, but **NC is
genuinely ambiguous for public-sector/charity distribution**. CC's NonCommercial clause turns on
the *primary purpose* of the use, and a council or NHS body re-publishing the output could be argued
either way. The plan should (a) state a default policy preferring **Mulberry (BY-SA)** precisely to
keep outputs commercially reusable by *any* downstream public body, and (b) record a per-symbol
provenance note (ARASAAC mixes its own work with third-party contributions — verify each pictogram,
not just the set).

**1.5 Accessibility & open-pictogram scope — correct and well-bounded.** Making the *file* accessible
(real text, tagged HTML/PDF-UA, alt text, reading order) a separate gate is right; image-only scanned
PDFs are the most common real-world Easy Read failure. Scope discipline ("not a portal," "not an
on-demand simplifier," "not cross-language") is healthy. **Gap:** alt-text strategy is unstated — an
Easy Read symbol beside text needs alt text that does **not** merely repeat the sentence (which
creates screen-reader redundancy) and may sometimes be decorative (`alt=""`). Coordinate with the
sibling `a11y-alttext-commons`. Also missing: a stance on **plain-language readability metrics** —
the plan should explicitly note that formulas (Flesch-Kincaid etc.) are *weak proxies* for
intellectual-disability comprehension and must never substitute for reader-validation.

**1.6 Completeness / honesty.** The "no partner, no validators secured ⇒ `verifiedNeed=false`,
nothing reaches Shipped" disclosure is exemplary and the M0/M1-are-partner-independent sequencing is
the right de-risking. The two most consequential *correctness* findings: **(A)** co-production is
declared the cardinal gate yet is entirely unstaffed and inserted only post-draft — the plan should
move reader involvement upstream and treat validator recruitment as a *parallel M0* activity, not an
M1 one, since without it M0's pilot cannot actually be reader-validated (the M0 DoD currently
*requires* reader-validation but validators are an M1 deliverable — an internal sequencing
inconsistency). **(B)** the symbol-license model should default to Mulberry/BY-SA and resolve the NC
ambiguity rather than leaving it to per-document Open Question 3, because the choice cascades into
every downstream partner's ability to reuse.

---

## 2. Competitive landscape (web-grounded)

| Org / tool | What it is | Strengths | Weaknesses / gaps `easy-read-plus` exploits |
|---|---|---|---|
| **Inclusion Europe — *Information for All*** ([link](https://www.inclusion-europe.eu/easy-to-read-standards-guidelines/)) | The European Easy-to-Read standard + logo/trademark | Authoritative standard; multilingual; rights-based; logo signals validation | A *standard*, not a content factory or pipeline; no open tooling, no symbol set, no provenance/licensing automation |
| **Photosymbols + EasyMaker** ([link](https://www.photosymbols.com/)) | Proprietary photo image-bank + Easy Read authoring tool | Huge, high-quality real-people photos; fair-pay model for its models; mature tooling | **Proprietary/subscription**; £3,500 extended-use to publish for others; outputs locked to license; not open/forkable |
| **CHANGE** ([link](https://www.changepeople.org/)) | Learning-disability charity; Easy Read service + image bank; "Words to Pictures" checker team | Gold-standard **co-production** (checkers with learning disabilities); deep expertise | Picture bank is **subscription/proprietary**; bespoke service (not open content commons); UK-centric |
| **Mencap Easy Read library** ([link](https://www.mencap.org.uk/easy-read-library)) | Large free library of co-produced Easy Read docs | Free to view/download; co-produced; trusted brand | Mencap's *own* topics only; not a reusable pipeline/spec; licensing of reuse/derivatives unclear; no dyslexia track |
| **Widgit** ([link](https://www.widgit.com/symbol-services/licensing.htm)) | Widgit Symbols + InPrint/SymWriter authoring | Symbol coverage; widely used in UK schools/services | **Proprietary** symbols + paid software; lock-in; output reuse constrained by license |
| **ARASAAC** ([link](https://arasaac.org/)) | ~10k+ open pictograms (CC BY-NC-SA) | Free, vast, multilingual, widely adopted | **NC clause** limits commercial/ambiguous public-sector reuse; symbols only (no Easy Read content, no pipeline) |
| **Mulberry Symbols** ([link](https://mulberrysymbols.org/)) | Open symbol set (CC **BY-SA**, commercial OK) | Truly open incl. commercial; SVG/vector; AAC-oriented | Smaller set than ARASAAC; symbols only; less "concrete public-information" coverage |
| **Global Symbols** ([link](https://globalsymbols.com/)) | Aggregator/host of open symbol sets + tools | Per-set licensing visible; multilingual; board-builder | Aggregator, not a content producer; license **varies per set** (must verify each) |
| **Books Beyond Words** ([link](https://booksbeyondwords.co.uk/)) | Wordless picture story books, co-created with lived experience | Strong co-production; evidence in health/education; trauma/relationship topics | Wordless narrative format (not text+symbol Easy Read); proprietary books; narrow to story genres |
| **PLAIN / plainlanguage.gov / ISO 24495** ([PLAIN](https://plainlanguage.com/); [gov](https://plainlanguage.gov/)) | Plain-language guidelines + ISO standard | Authoritative method basis; gov-mandated in US | Plain language ≠ Easy Read (no symbols, not ID-specific, no co-production requirement) |
| **Generic LLM "simplifiers"** (HyperWrite etc.) | One-click AI text simplification | Fast, cheap, scalable | **No provenance, no symbol licensing, no co-production, no fidelity gate** — exactly the unsafe pattern the plan rejects ([evidence of meaning-loss](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11795904/)) |

**Cross-cutting market reality:** the *evidence base for Easy Read effectiveness is weak/mixed* —
systematic reviews find insufficient high-quality evidence that Easy Read improves comprehension or
behavior change, and illustration effects are mixed
([JPPI review](https://onlinelibrary.wiley.com/doi/10.1111/jppi.12201);
[JARID 2026 review, PMC](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12893875/)). This is both a
risk (don't over-claim) and an *opportunity*: a project that systematically logs reader-comprehension
outcomes would contribute rare evidence to the field.

---

## 3. Gaps we can fill

1. **An open, forkable Easy Read content commons** — every major content producer (Photosymbols,
   CHANGE, Mencap-for-its-own-topics, Widgit) is proprietary, bespoke, or single-org. There is no
   openly-licensed, provenance-complete, reusable corpus of adapted essential-information.
2. **License-as-data automation across three layers** — nobody publishes a checkable, derived
   output-license model spanning source + symbols + fonts. This is a genuine, defensible novelty.
3. **Open-symbol-only Easy Read** — most existing Easy Read depends on Widgit/Photosymbols/PCS.
   Producing equivalent quality using only ARASAAC/Mulberry/Global-Symbols is an unmet, demonstrable
   public good.
4. **Co-production + open content together** — CHANGE/Mencap co-produce but don't release a reusable
   pipeline; ARASAAC/Mulberry are open but produce no co-produced *content*. The intersection is
   empty.
5. **Reader-comprehension evidence** — given the thin evidence base, a disciplined comprehension log
   across documents is a research-grade contribution few producers maintain.
6. **A dyslexia-friendly track from the same source** — Easy Read producers rarely also ship a
   BDA-aligned dyslexia-friendly variant from one meaning-checked core. Two-outputs-one-source is
   differentiated.
7. **Output-file accessibility discipline** — much existing Easy Read is image-only PDF that defeats
   screen readers; shipping tagged HTML + PDF-UA is a concrete quality gap to fill.

---

## 4. Differentiators to win

1. **Open symbols + open output, end to end** — the only approach where source, symbols (ARASAAC/
   Mulberry), fonts (OFL), *and* the derived output license are all open and attributed. No £3,500
   extended-use trap; any public body can reuse.
2. **Co-production as a hard, fairly-paid gate** (not "user testing") — matches CHANGE/Mencap ethics
   but in a *reusable, openly-licensed* package, with consent + fair pay + no-PII designed in.
3. **License-derived-not-asserted, machine-checked** — the three-layer manifest → required-output-
   license computation (`license-002`) is a defensible engineering moat competitors lack.
4. **Translation-grade meaning-fidelity gate** — `UNCERTAIN:` flags that *block* sign-off, borrowed
   from `vital-info-translations`; turns "AI simplification" from unsafe to auditable.
5. **Two outputs, one meaning-checked core** (Easy Read + dyslexia-friendly) — broader reach per
   unit of fidelity work.
6. **Evidence honesty** — explicitly refuses "dyslexia font cures dyslexia" and over-claims about
   Easy Read efficacy; cites the mixed evidence. Trust differentiator with expert partners.

---

## 5. Claude API leverage — and where Claude must NOT decide

**High-value, human-gated uses (drafting aids that accelerate co-production, never replace it):**
- **First-draft plain-language simplification** of an allow-listed source into the Easy Read style
  spec (short sentences, one idea per line, common words), emitted *with* `UNCERTAIN:` fidelity flags
  for every dropped condition/negation/caveat. Claude is fast and competent at sentence-level
  simplification ([benchmark](https://www.aimodels.fyi/papers/arxiv/redefining-simplicity-benchmarking-large-language-models-from)),
  which is exactly the layer it does well.
- **Symbol *suggestion* (not selection)** — propose candidate ARASAAC/Mulberry pictograms per idea
  *by stable id from the allow-list*, with a confidence/`symbol-fit` flag, for a human + reader to
  confirm. Claude proposes; co-production disposes.
- **Meaning-fidelity *assistant*** — generate a structured source-vs-draft diff highlighting
  conditions/negations/"only if" clauses for the human reviewer to verify; draft glossary entries for
  unavoidable hard words.
- **Readability + structural lint** — flag long sentences, rare words, passive voice, missing alt
  text (advisory signal feeding `readability-001`), explicitly *not* a pass/fail substitute for
  reader-validation.
- **Back-translation check** — restate the Easy Read in plain prose so a reviewer can compare to the
  source intent (a sensitive, cheap meaning-loss detector).
- **Dyslexia-friendly structural pass** — apply BDA layout/structure to the *same* meaning-checked
  content.

**Where Claude must NOT decide (hard lines, matching plan + guardrails):**
- **Co-production is mandatory and human** — Claude output is never reader-validation. No AI-only
  Easy Read ships, full stop. People with intellectual disabilities are the cardinal gate.
- **Accuracy-when-simplified is human-verified** — a human reviewer must sign off against the source;
  unresolved `UNCERTAIN:` flags block "done." Claude may flag, never clear.
- **Symbol/source/font licensing is human-verified** — Claude may *draft* allow-list entries but a
  human license reviewer confirms derivative permission, NC/SA flags, and the derived output license
  (`license-000` is a written, human gate). Use **ARASAAC/Mulberry open sets only**.
- **High-stakes domains** (medical/legal/safety/benefits/financial) require credentialed-expert
  sign-off and verbatim "information, not advice" framing — never Claude alone.
- **No headless/automated lane in v0.1** — donated lane only; the human runs their own agent.

---

## 6. Ten concrete optimizations

1. **Move co-production upstream.** Add a pre-draft "reader co-design / priority-setting" touchpoint
   (consultants at the start, checkers at the end) per NHS England guidance — don't insert readers
   only at step 7.
2. **Resolve the M0 sequencing inconsistency.** M0's DoD requires reader-validation, but validators
   are an M1 deliverable. Either pull "recruit ≥1 validator/advocacy route" into M0, or relabel the
   M0 pilot as "validation-ready, validated in M1."
3. **Default to Mulberry (BY-SA); make ARASAAC opt-in.** Pin the exact CC version per symbol set and
   resolve the NC-ambiguity policy now, so outputs stay reusable by any downstream public body.
4. **Per-pictogram provenance, not just per-set.** ARASAAC contains third-party-contributed symbols;
   record license at the symbol level in `assets-manifest.yaml`.
5. **Adopt an explicit alt-text policy** (don't duplicate the adjacent sentence; mark purely
   decorative symbols `alt=""`); coordinate with `a11y-alttext-commons`.
6. **Add a back-translation / round-trip comprehension check** to the fidelity gate — more sensitive
   than the checklist to silent meaning-loss.
7. **State that readability formulas are advisory only.** Bake into `spec-001`/`readability-001` that
   Flesch-Kincaid etc. never substitute for reader-validation (the evidence base supports this).
8. **Reconcile house style with easyreadstandard.org and the Inclusion Europe logo rules** so
   reviewers audit against a single named basis, and record logo-eligibility.
9. **Publish a reader-comprehension dataset** (de-identified, consented) as a standing output — rare
   field evidence and a credibility asset with partners and academics.
10. **Define validation sample/method concretely** (how many readers, recruitment route, comprehension
    instrument, "pass" threshold) in `validate-001`, so "reader-validated" is auditable, not vibes.

---

## 7. Parallel & perpendicular spin-offs

- **`open-pictograms` (parallel).** A curated, license-clean, machine-readable index of ARASAAC +
  Mulberry + Global-Symbols *with per-symbol license + derived-output-license data* — directly the
  asset allow-list, generalized into a shared dependency for the whole portfolio.
- **`vital-info-translations` / `health-info-translations` (perpendicular).** Cross-language Easy
  Read = both projects' gates combined (meaning fidelity × language fidelity × symbol licensing).
  Shared `UNCERTAIN:`-flag and provenance machinery already mirror each other.
- **`know-your-rights` (parallel).** Rights/benefits content is high-value Easy Read territory but
  high-stakes — feeds this project's credentialed-expert gate; the two should share a risk-domain
  classifier.
- **`benefits-navigator` (perpendicular).** Benefits notices are a flagship Easy Read use case;
  combine with this pipeline for accessible, accurate, co-produced benefit explanations (high-stakes
  ⇒ expert sign-off).
- **Reusable Easy Read pipeline (parallel, internal).** The specs + checklists + schemas + license-
  derivation tooling, packaged as a standalone, agent-neutral toolkit any Elyos content project can
  ride.
- **An MCP server (perpendicular).** A "cognitive-accessibility" MCP exposing: source-allow-list
  lookup, open-symbol search by concept (returning stable ids + license + derived-output-license),
  readability/structural lint, and a meaning-fidelity diff — so *any* agent can draft compliant Easy
  Read drafts for human/co-production review, with licensing enforced at the tool boundary.

---

## 8. Open questions

1. **NC vs BY-SA default** — adopt Mulberry/BY-SA project-wide for reuse, accepting ARASAAC's larger
   set only when concept coverage demands it (and the output is declared NC-SA)? (Plan's Open Q3,
   elevated.)
2. **Where does co-production start?** Pre-draft co-design *and* post-draft validation, or validation
   only? Who pays, at what rate, recruited via which advocacy/self-advocacy route?
3. **Validator gate vs M0 timing** — can M0 honestly ship a "reader-validated" pilot before M1
   recruits validators? (Sequencing fix needed.)
4. **Inclusion Europe logo** — do we pursue formal European Easy-to-Read logo eligibility, and does
   our validation method satisfy it?
5. **High-stakes in v0.1?** — stay standard-tier until the credentialed-expert network exists, or
   take one high-stakes document with sign-off as a proof?
6. **Evidence contribution** — do we commit to publishing comprehension outcomes given the field's
   thin evidence base, and under what consent model?
7. **Alt-text division of labor** with `a11y-alttext-commons`, and symbol-vs-alt-text redundancy
   policy.
8. **Partner sourcing** — still the binding external constraint (Plan Open Q1); pause/pivot decision
   if none by end of M1.

---

## Summary (for the maintainer)

**Top 3 competitors.** (1) **Photosymbols** — dominant proprietary Easy Read image bank + EasyMaker
tooling, but subscription-locked and charges £3,500 "extended use" to publish for others; (2)
**CHANGE** — gold-standard co-production via its learning-disability checker team, but a subscription
picture bank and bespoke service, not an open commons; (3) **Mencap's Easy Read library** — large,
free, co-produced, but only Mencap's own topics with unclear reuse/derivative terms. Adjacent: ARASAAC
(open but NC), Mulberry (fully open BY-SA), Inclusion Europe (the standard, not a producer), Widgit/PCS/
SymbolStix (proprietary symbols), Books Beyond Words (wordless, proprietary), and generic LLM
simplifiers (no provenance/licensing/co-production — the unsafe pattern the plan rightly rejects).

**Single strongest differentiator.** An **open-symbol + open-output Easy Read commons gated by paid
co-production** — the only offering where source, symbols, fonts, *and* the machine-derived output
license are all open and attributed, while still validated *with* people with intellectual
disabilities. Nobody else occupies the open-content × co-production intersection.

**Top 3 Claude-API ideas.** (1) First-draft simplification into the Easy Read spec that *emits
blocking `UNCERTAIN:` fidelity flags* for every dropped condition/negation/caveat; (2) open-symbol
*suggestion* by stable allow-list id with a `symbol-fit` flag, for human + reader confirmation; (3) a
meaning-fidelity diff + back-translation check that surfaces silent meaning-loss for the human
reviewer. All three accelerate humans; none decides.

**Two most important plan-correctness findings.** (A) **Co-production is named the cardinal gate but
is structurally under-built:** readers appear only at post-draft validation (best practice also
engages them up front), and M0's Definition of Done *requires* reader-validation while validators are
an M1 deliverable — an internal sequencing inconsistency that must be resolved (pull validator
recruitment into M0, or relabel the M0 pilot "validation-ready"). (B) **Symbol-licensing needs a
decided default:** the plan's facts are correct (ARASAAC = CC BY-NC-SA, Mulberry = CC BY-SA,
Photosymbols/Widgit/PCS/SymbolStix/CHANGE = proprietary), but the **NonCommercial clause is genuinely
ambiguous for public-sector/charity reuse**, so the project should default to **Mulberry/BY-SA**, pin
exact CC versions, and record license at the *per-pictogram* level — rather than leaving the cascading
choice to per-document discretion.
