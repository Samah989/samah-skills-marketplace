---
name: systematic-literature-review
description: Guide a user through a rigorous systematic literature review or scoping review following PRISMA 2020, from protocol through search-string design, deduplication, title/abstract and full-text screening prompts, data extraction template, PRISMA flow diagram, and risk-of-bias assessment. Trigger this skill whenever the user mentions systematic review, scoping review, PRISMA, meta-analysis, literature review methodology, search strategy, PICO/PECO, screening protocol, Rayyan, Covidence, Endnote deduplication, risk of bias, ROBINS, or asks how to review the literature on a topic rigorously. Also trigger for requests like "help me review the literature on X," "design a search for a systematic review," "how do I dedupe my search results," or "what's the difference between a systematic and a scoping review."
---

# Systematic Literature Review

Author: Samah Alshrief · https://github.com/Samah989

Guide a rigorous evidence-synthesis project  -  systematic review, scoping review, or rapid review  -  from protocol through reporting. This skill is grounded in PRISMA 2020 (the standard reporting checklist for systematic reviews) and the JBI methodology for scoping reviews.

## When to use this skill

- The user says "systematic review," "scoping review," "meta-analysis," "literature review," "PRISMA," "PICO," "search strategy," "screening protocol"
- The user needs help *designing* a search string, dedup workflow, screening prompts, or a data extraction template
- The user has a pile of exported records (RIS/BibTeX/CSV from PubMed, Web of Science, Scopus, OpenAlex, Google Scholar) and needs a reproducible workflow
- The user asks the difference between a systematic and a scoping review, or which is appropriate for their question
- The user is preparing a PRISMA flow diagram
- The user is at any stage of the SAAS workflow (Search-Appraisal-Analysis-Synthesis)

## Guiding principles

**A systematic review is a methodology, not a size.** The distinguishing feature is not "reviewed a lot of papers" but "specified the protocol *before* looking, executed reproducibly, and reported every choice." A rigorous review of 30 papers with a pre-registered protocol beats an unstructured summary of 300.

**Report every choice.** PRISMA 2020's biggest ask is transparency: which databases (with dates and versions), the exact search strings run in each, the deduplication rules, which two reviewers screened (and how disagreements were resolved), and what was included or excluded and why. If a reader can't rerun your search and land on the same numbers, the review isn't systematic.

**Match the review type to the question.**
- **Systematic review**: a focused, answerable question (usually PICO/PECO); synthesizes evidence to answer it. Often paired with a meta-analysis when studies are quantitatively poolable.
- **Scoping review**: a broader, mapping question  -  "what does the literature look like on X?"  -  good for emerging fields, follows the JBI or Arksey & O'Malley methodology.
- **Rapid review**: a systematic review with pre-declared shortcuts (e.g., one reviewer, limited databases) for time pressure.
- **Umbrella review**: a review of existing systematic reviews.

Pick the wrong type and the whole thing wobbles.

## Workflow

### 1. Frame the question

Ask the user to state their question in a structured framework. Use whichever fits:

- **PICO** (clinical/interventional): Population, Intervention, Comparator, Outcome
- **PECO** (exposure/observational): Population, Exposure, Comparator, Outcome
- **SPIDER** (qualitative research): Sample, Phenomenon of Interest, Design, Evaluation, Research type
- **PCC** (scoping reviews, JBI): Population, Concept, Context

Rewrite the user's fuzzy question in the chosen framework and confirm before proceeding. This shapes every downstream decision.

### 2. Draft the protocol

Produce a short protocol document with:

1. **Review question** (in framework form) + review type
2. **Rationale** (2-3 sentences)
3. **Eligibility criteria**  -  inclusion + exclusion, each with a justification
4. **Information sources**  -  the databases you'll search, plus grey literature and citation-tracking plans
5. **Search strategy**  -  the concept blocks, synonyms, and the exact string for each database
6. **Selection process**  -  number of reviewers, screening stages, tie-breaker
7. **Data extraction plan**  -  the fields you'll pull from each included study
8. **Risk-of-bias tool**  -  Cochrane RoB 2 for RCTs, ROBINS-I for non-randomized, QUADAS-2 for diagnostic accuracy, MMAT for mixed methods
9. **Synthesis approach**  -  narrative, meta-analytic (with model choice), or scoping-style mapping
10. **Deviations log**  -  a place to record any protocol changes and why

Recommend registering the protocol at PROSPERO (systematic reviews) or OSF (scoping / non-clinical reviews) *before* running searches. Registration is now a common requirement of high-quality journals.

### 3. Design the search strategy

The heart of the review. Build one concept block per PICO element, then AND them together.

**Method  -  for each concept:**

1. Identify controlled-vocabulary terms per database (MeSH in PubMed, Emtree in Embase, subject headings in CINAHL).
2. Add free-text synonyms (title/abstract), including spelling variants, plurals, and truncation.
3. OR within a block; AND across blocks.
4. Add limiters last (date, language, publication type) and note each one  -  every limiter *reduces reproducibility risk* but *introduces bias*.

**Example  -  CRISPR + cardiovascular disease + humans:**

Concept 1 (CRISPR):
```
"CRISPR-Cas Systems"[MeSH] OR "CRISPR"[tiab] OR "Cas9"[tiab] OR "gene editing"[tiab]
```
Concept 2 (cardiovascular disease):
```
"Cardiovascular Diseases"[MeSH] OR "heart disease*"[tiab] OR "cardiovascular"[tiab]
```
Combined:
```
(concept 1) AND (concept 2) AND humans[MeSH] AND English[la]
```

Rewrite the exact syntax for each additional database. PubMed, Scopus, Web of Science, Embase, and OpenAlex all differ in Boolean nesting, wildcard characters, and field-tag syntax  -  the search string is *not* portable and must be re-authored per database.

**Cover free/open sources too**  -  OpenAlex (open scholarly graph, no paywall), Semantic Scholar, and Europe PMC. These often surface preprints and grey literature the paywalled databases miss.

### 4. Dedupe and stage for screening

After exporting records from each database:

1. Combine into one library (RIS or CSV).
2. Deduplicate. Standard rule: match on DOI first; fall back to normalized title + first author + year. Zotero, EndNote, and Rayyan all have dedupe features; expect to hand-check ~5% for edge cases.
3. Randomly assign a small pilot batch (~5% of records) to both reviewers to calibrate the eligibility criteria before committing to full screening.
4. Load the deduplicated set into a screening tool  -  free options are Rayyan and ASReview; paid options are Covidence and DistillerSR.

### 5. Title/abstract screen

For each record, two reviewers independently answer: **include, exclude, or unclear.** Track exclusion reasons using the same short list from the protocol (e.g., "wrong population," "wrong outcome," "conference abstract only").

Resolve disagreements by discussion. If discussion fails, a third reviewer decides. Report the inter-rater agreement (Cohen's kappa) after the pilot; > 0.6 is acceptable, > 0.8 is strong.

### 6. Full-text screen

Retrieve full text for everything not excluded at title/abstract. Exclude with a specific reason per record. Every excluded full-text article must have its reason logged and reported in the PRISMA flow diagram.

### 7. Data extraction

Build a structured extraction template *before* extracting. Fields depend on the question but typically include: bibliographic metadata, study design, setting, sample size, population characteristics, intervention/exposure detail, comparator, outcomes with effect sizes and confidence intervals, funding source, and conflicts of interest.

Two reviewers extract independently for a sample (~20%); reconcile differences; one reviewer completes the rest with the second checking.

### 8. Risk of bias

Apply the appropriate tool per study design:

| Study design | Tool |
|--------------|------|
| Randomized controlled trials | Cochrane Risk of Bias 2 (RoB 2) |
| Non-randomized interventions | ROBINS-I |
| Observational (cohort, case-control) | ROBINS-E or Newcastle-Ottawa Scale |
| Diagnostic accuracy | QUADAS-2 |
| Prognostic factor studies | QUIPS |
| Mixed methods | MMAT |

Report per-study RoB in a summary figure (traffic-light plot).

### 9. Synthesis

- **Meta-analysis** where studies are quantitatively poolable  -  same outcome, same effect measure, and reasonable clinical/methodological homogeneity. Choose fixed-effect vs random-effects deliberately (random-effects is safer when heterogeneity is expected).
- **Narrative synthesis** otherwise. Use structured methods (SWiM, guidance from the Cochrane Handbook Chapter 12), not "authors' impressions."
- **Scoping synthesis**  -  map results by concept, population, methodology, year; typically no meta-analysis.

Assess certainty of the body of evidence with GRADE for each key outcome.

### 10. PRISMA flow diagram

Every systematic review reports a flow diagram counting records at each stage. The 2020 update separates database vs other sources and adds fields for automation tools. Use the PRISMA-flow-diagram generator at <https://www.prisma-statement.org/prisma-2020-flow-diagram>.

Numbers to report:
- Records identified per source (databases + registers + citation searching + other)
- Records removed before screening (duplicates, marked ineligible by automation, other)
- Records screened at title/abstract, excluded
- Reports sought for retrieval, not retrieved (with reasons)
- Reports assessed for eligibility at full text, excluded (with reasons)
- Studies included in the review, plus studies included in the meta-analysis if any

### 11. Reporting

Use the PRISMA 2020 checklist (27 items) as your final QA pass. For scoping reviews use PRISMA-ScR. For network meta-analyses use PRISMA-NMA. Include the completed checklist as an appendix.

## Standard artifacts to produce

Whenever you complete a review with a user, generate these files by default:

1. `protocol.md`  -  the pre-registered protocol
2. `search_strategy.md`  -  full syntax per database with dates run
3. `included_studies.csv`  -  final included set with all extracted fields
4. `excluded_full_text.csv`  -  every full-text exclusion with reason
5. `prisma_flow.md`  -  the flow-diagram counts (plus a link to a rendered SVG/PDF)
6. `risk_of_bias.csv`  -  per-study RoB judgments
7. `prisma_checklist.csv`  -  completed 27-item checklist

## Tools worth knowing

- **Registration**: PROSPERO (clinical), OSF Registries (any)
- **Search**: PubMed, Embase, Scopus, Web of Science, OpenAlex, Semantic Scholar, Europe PMC, Cochrane Library, CINAHL
- **Deduplication**: Zotero, EndNote, Rayyan, ASReview
- **Screening**: Rayyan (free), ASReview (free, ML-assisted), Covidence (paid), DistillerSR (paid)
- **Extraction**: SR-DATA, Covidence, custom Google Sheet
- **Risk of bias**: Cochrane RoB tools (rob.cebm.ox.ac.uk), robvis for visualization
- **Meta-analysis**: R meta / metafor packages, Stata metan, RevMan (Cochrane), OpenMeta[Analyst]
- **Reporting**: PRISMA 2020 checklist + flow generator at prisma-statement.org

## Tone and delivery

- Recommend registering the protocol before searching  -  that's the single biggest quality lever.
- Push back gently on scope creep. Reviews that grow from three concepts to seven usually fail to finish.
- If the user's timeline is short, be honest: a proper systematic review typically takes 6-18 months. Suggest a scoping or rapid review with pre-declared shortcuts instead of a compromised systematic review.
- When quality is at stake, cite the standard: "PRISMA 2020 item 7 asks for the exact search string. Let's write yours."
