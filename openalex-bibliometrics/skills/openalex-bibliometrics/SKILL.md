---
name: openalex-bibliometrics
description: Run bibliometric and scientometric analyses using OpenAlex  -  the free, open scholarly graph covering 260M+ works, 100K+ institutions with ROR IDs, 250K+ authors with ORCID linkage, and every citation edge. Use for institution profiles, author profiles, citation counts and h-indexes, co-authorship networks, funder tracking, field-of-research trends, journal comparisons, and OA-share analysis. Free alternative to Scopus and Web of Science. Trigger this skill whenever the user mentions OpenAlex, bibliometrics, scientometrics, h-index, citation analysis, author metrics, institutional research output, funder tracking, ROR, ORCID, Concept tagging, or asks for "how many papers has <author/institution> published," "what's the citation impact of <topic>," "who cites <paper>," "OA rate for <journal/institution>," or "co-authors of <person>."
---

# OpenAlex Bibliometrics

Author: Samah Alshrief · https://github.com/Samah989

Answer bibliometric and scientometric questions using OpenAlex  -  the free, open scholarly graph launched in 2022 as a successor to Microsoft Academic Graph. Replaces paywalled Scopus/Web of Science for the vast majority of institutional research and funder-tracking use cases.

## When to use

- "How many papers has <author or institution> published?"
- "What's the h-index / citation count of <person>?"
- "Who are <person>'s top co-authors?"
- "What's the OA share at <journal / institution / field>?"
- "Which funders support work on <topic>?"
- "How has research on <concept> grown over time?"
- Requests naming OpenAlex, DOI, ORCID, ROR, Concept, Topic (OpenAlex's 2024 taxonomy)

## Data source

OpenAlex  -  <https://openalex.org>

- Free REST API, no key required. Polite pool of 100K requests/day if you include an email in the `mailto=` param; premium at higher volume.
- Base: `https://api.openalex.org/`
- Docs: <https://docs.openalex.org/>
- Snapshot for bulk: monthly dump on AWS S3 (`s3://openalex/`).

Core entities:

| Entity | Path | ID prefix | Description |
|--------|------|-----------|-------------|
| Works | `/works` | W | Papers, books, datasets, preprints (260M+) |
| Authors | `/authors` | A | Disambiguated authors, many with ORCID (250M+) |
| Institutions | `/institutions` | I | Universities and research orgs with ROR IDs (100K+) |
| Sources | `/sources` | S | Journals, conferences, repositories (250K+) |
| Publishers | `/publishers` | P | Elsevier, Springer Nature, etc. |
| Funders | `/funders` | F | NIH, NSF, ERC, Wellcome, etc. |
| Concepts | `/concepts` | C | Legacy hierarchical topic taxonomy |
| Topics | `/topics` | T | 2024 replacement for Concepts  -  cleaner, ~4,500 topics |

## Common queries

### Author profile

```
GET https://api.openalex.org/authors/orcid:0000-0002-1825-0097?mailto=you@example.com
```
Returns works count, cited-by count, h-index, i10-index, institution history, top concepts, works URL.

### Institution profile

```
GET https://api.openalex.org/institutions/ror:https%3A%2F%2Fror.org%2F04dkp9463
```
(Seton Hall University = `ror:https://ror.org/04dkp9463`). Returns works count, cited-by, top concepts, associated authors.

### Filtered works search

```
GET /works?filter=institutions.ror:https://ror.org/04dkp9463,publication_year:2020-2024,is_oa:true&per-page=200&mailto=you@example.com
```
Every filter can be combined with `,`. Group with `group_by=` for aggregations.

### Aggregation examples

- Works per year by an institution: `filter=institutions.id:I<id>&group_by=publication_year`
- Journals an institution publishes in most: `filter=institutions.id:I<id>&group_by=primary_location.source.id`
- OA share by year: `filter=institutions.id:I<id>&group_by=publication_year,is_oa`
- Top collaborating institutions: `filter=authorships.institutions.id:I<id>&group_by=authorships.institutions.id`

## Workflow

### 1. Resolve the entity
Given a user string ("Samah Alshrief," "Seton Hall University," "The Lancet"):
1. Search: `/authors?search=Samah+Alshrief` or `/institutions?search=Seton+Hall`.
2. Confirm the top hit with the user before drilling  -  OpenAlex author disambiguation is good but not perfect, especially for common names.
3. Prefer resolving via a canonical ID when possible (ORCID for people, ROR for institutions, DOI for works, ISSN-L for journals).

### 2. Choose the granularity
- **Single-entity summary**: fetch the entity's JSON, extract h-index, works count, cited-by, OA share, top concepts/topics.
- **Trend analysis**: use `group_by=publication_year` on a filtered work list.
- **Network analysis**: fetch works filtered by author or institution, then aggregate co-authors from `authorships` blocks.
- **Cross-entity comparison**: run one API call per entity, tabulate side by side.

### 3. Report with context
- Always cite the OpenAlex ID and the snapshot date  -  OpenAlex data updates monthly.
- Distinguish `cited_by_count` (total citations) from `2yr_mean_citedness` (recent impact).
- Note when a count seems suspicious (very new author with 500 works = author disambiguation error).

## Field-specific caveats

- **Author disambiguation**: OpenAlex clusters papers to authors via ML. Cluster quality varies; ORCID-linked authors are best. Common Chinese, Indian, and Arabic names have higher error rates.
- **Institution counts**: institution attribution is inferred from affiliation strings; small institutions can be undercounted, and multi-affiliation authors count once per affiliation.
- **OA classification**: OpenAlex categorizes as gold / green / hybrid / bronze / closed. `is_oa=true` covers all non-closed; be explicit about which OA definition matters for the question.
- **Concept vs Topic**: Concepts (C IDs) are the legacy 19-level hierarchical taxonomy  -  being phased out. Topics (T IDs) are the 2024 replacement  -  flatter, cleaner, ~4,500 topics tied to 26 subfields. Prefer Topics for new work.

## Comparison to alternatives

| Source | Strengths | Limitations |
|--------|-----------|-------------|
| **OpenAlex** | Free, open license (CC0), huge, ORCID + ROR native, monthly updates | Author disambiguation imperfect; institution attribution heuristic |
| **Scopus** | Curated, strong metadata, mature APIs | Paywalled ($$$$); no institutional bulk without subscription |
| **Web of Science** | Curated, journal-impact-factor source | Paywalled; smaller coverage than Scopus and OpenAlex |
| **Semantic Scholar** | Free, strong NLP-based extraction | Smaller than OpenAlex; less institutional detail |
| **Google Scholar** | Broadest coverage | No API; scraping violates ToS; questionable metadata quality |
| **Dimensions** | Rich funder/patent links | Freemium; paywalled for bulk |

For virtually every task an institutional researcher, bibliometrics librarian, or grants office needs, OpenAlex is now the right default.

## Output formats

- **Single entity**: profile card with name, ID, works, citations, h-index, top 5 topics, OA share, most-cited work.
- **Comparison**: Markdown table, entities as rows, metrics as columns.
- **Time series**: table + a description of the trend (growth/plateau/decline, notable inflection years).
- **Network summary**: top 10 collaborators or top 10 topics with counts.

## Tone

- Cite OpenAlex IDs so answers are auditable and rerunnable.
- Distinguish "author has 47 papers" from "OpenAlex clusters 47 papers to this author," especially for common names.
- Be explicit about OA definitions  -  different funders and mandates mean different things by "open access."
- If a user asks a question OpenAlex can answer but the underlying data has known quality issues (e.g., citation counts for a very recent paper), note the caveat.
