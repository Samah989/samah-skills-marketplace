---
name: data-management-plan
description: Generate a funder-compliant Data Management Plan (DMP) for a research project. Supports every major funder template  -  NSF (2026 DMSP), NIH (Data Management and Sharing Policy 2023+), Wellcome, ERC, Horizon Europe, MRC, NSERC, ARC  -  and produces the plan as a Word or Markdown document with every required section filled in from a short project brief. Trigger this skill whenever the user mentions data management plan, DMP, DMSP, data sharing policy, NSF DMP, NIH DMS, Wellcome data policy, Horizon Europe data management, FAIR data, open data mandate, data repository selection, or asks about their grant's data-management requirements. Also trigger for requests like "write my DMP," "what does NIH require for data sharing," "which repository should I use for this dataset," or "is my project FAIR-compliant."
---

# Data Management Plan

Author: Samah Alshrief · https://github.com/Samah989

Generate a funder-compliant Data Management Plan (DMP)  -  the document nearly every research grant now requires  -  from a short project brief. Covers the templates and section requirements of the major funders, plus best practices for FAIR data, repository selection, and cost estimation.

## When to use this skill

- The user says "DMP," "data management plan," "data sharing plan," "DMSP" (NIH's 2023+ policy), "data management and sharing plan"
- The user is preparing a grant proposal for NSF, NIH, Wellcome, ERC, Horizon Europe, MRC, NSERC, ARC, or any funder with a DMP requirement
- The user asks about FAIR data (Findable, Accessible, Interoperable, Reusable), open data mandates, or data-sharing compliance
- The user needs help picking a data repository (Zenodo, Dryad, Figshare, ICPSR, GEO, dbGaP, OpenNeuro, etc.)
- The user asks how much to budget for data-management or -sharing costs

## Guiding principles

**A DMP is a live document, not a compliance box-check.** Funders in 2026 increasingly require *evidence* of data-management activity mid-grant  -  not just a plan filed at submission. Structure the DMP so the user can update it as the project evolves, and encourage them to use a machine-actionable tool like DMPTool.org or Argos so the plan can be revisited.

**Match the template to the funder.** Every funder has its own required sections and its own preferred format. Do not paste a generic DMP into an NSF proposal; NSF is looking for specific wording aligned with their DMSP guidance. See the templates section below.

**FAIR ≠ open.** FAIR (Findable, Accessible, Interoperable, Reusable) does not require data to be public. Human-subjects data, genomic data, and proprietary industrial data can be *accessible under controlled access* and still be FAIR. Explain this to users who are worried about privacy.

**Budget for it.** Data-management labor, storage, and repository deposit fees are legitimate direct costs on most funder budgets. NIH explicitly allows up to $10K/year in DMS costs on most awards. Include these in the DMP and in the budget justification.

## Workflow

### 1. Intake  -  collect the project brief

Ask the user for these pieces (in one message if possible):

1. **Funder + mechanism** (e.g., "NSF CAREER," "NIH R01," "Horizon Europe RIA," "Wellcome Discovery Award")
2. **Project title + one-paragraph aim**
3. **Data types**  -  what will be generated or collected (survey responses, sequencing reads, model outputs, interview transcripts, images, sensor logs, code, etc.)
4. **Expected volume** (rough  -  GB or TB, and file counts)
5. **Human-subjects or animal-subjects involvement**
6. **Anticipated sensitive elements** (PII, PHI, genomic, geolocated, proprietary)
7. **Team + institutional resources** (does the institution have a data repository? library data-services team? IRB?)
8. **Duration** of the grant

If the user is early and hasn't decided some of these, note them as TBD and generate the DMP with placeholders they can fill in later  -  but flag which items they must resolve before submission.

### 2. Pick the template

| Funder | Template / Policy | Length | Key required sections |
|--------|-------------------|--------|-----------------------|
| **NSF** | 2026 Data Management and Sharing Plan (DMSP) | 2 pages max | Data types; standards; policies for access & sharing; policies for reuse; archiving |
| **NIH** | Data Management and Sharing Policy (in effect Jan 2023) | 2 pages max | Data type; related tools; standards; data preservation, access, timelines; access, distribution, reuse considerations; oversight |
| **Wellcome** | Data, Software and Materials Management Plan | 1-2 pages | Data generated; standards & metadata; access & sharing; ethics & IP; long-term preservation |
| **ERC** | ERC Open Research Data policy | Deliverable D1.1 (DMP) | FAIR compliance narrative per dataset; allocation of resources; data security; ethical aspects |
| **Horizon Europe** | Standard European DMP template | Living DMP, updated in months 6, 18, final | Data summary; FAIR data; allocation of resources; data security; ethics; other |
| **MRC / UKRI** | UKRI data & software policy | 1 page | Data volume/type; sharing plans; timeline; controlled access rationale |
| **NSERC / SSHRC (Canada)** | Tri-Agency Research Data Management Policy | 2 pages | Data collection; documentation and metadata; storage and preservation; sharing and reuse; responsibilities and resources |
| **ARC (Australia)** | ARC RDM plan | 1-2 pages | Data description; storage; retention & sharing; ethics; responsibilities |

Confirm the funder + template with the user before drafting.

### 3. Draft the DMP  -  section-by-section prompts

For each section, ask the user only the question the section needs. Do not ask them to write prose; write it for them from their short answers.

#### Data type and format
- What data types will you generate? (list each  -  e.g., "sequencing reads (FASTQ)," "field survey responses (CSV)," "interview audio (WAV)")
- Which are pre-existing vs newly generated? Cite the sources of pre-existing data.
- Which formats will you use? Prefer open, non-proprietary formats (CSV over XLSX; TIFF over proprietary microscopy formats where possible).

#### Standards and metadata
- Which community metadata standards apply? (Dublin Core, DataCite, DDI for social science, MIAME for microarrays, MIxS for microbiome, etc.)
- Which controlled vocabularies / ontologies? (MeSH, SNOMED CT, ROR for institutions, ORCID for people)
- Where will the metadata live? (repository record, README in code repo, both)

#### Access, sharing, and reuse
- What will be shared and when? Distinguish: (a) analysis-ready datasets, (b) raw data, (c) code, (d) protocols.
- Public open access, or controlled access? If controlled: what's the access mechanism and who governs approvals?
- License? Datasets: CC0 or CC BY 4.0 are strongly preferred by most funders. Code: MIT, BSD-3, or Apache-2.0.

#### Repositories
- Deposit at least one **domain-appropriate repository**  -  see the picker matrix below.
- For general datasets: Zenodo (free, DOI-minting, tied to CERN), Figshare, or Dryad (curated).
- For code: GitHub with a Zenodo mirror for versioned DOIs.
- Cite the repository's re3data.org entry to signal you've done due diligence.

#### Preservation and long-term storage
- Committed to at least 10 years post-publication (NIH), or longer if required.
- Institutional archive + external repository is a strong belt-and-suspenders combo.

#### Security, sensitive data, and ethics
- IRB / IEC / ethics-committee approvals and how consent addresses data reuse.
- De-identification approach for human-subjects data (Safe Harbor, HIPAA Expert Determination, k-anonymity, differential privacy).
- Data access committees for controlled-access datasets.
- Encryption at rest and in transit.

#### Roles and responsibilities
- Data steward on the team (name a real person).
- Institutional Data Governance office and library data services if applicable.

#### Cost estimate
- Storage during the grant (institutional or cloud).
- Repository deposit fees (Dryad ~$120/dataset; most others free).
- Data-management labor (fraction of a data manager's or postdoc's FTE).
- Software licenses for essential tools.

### 4. Output

Produce two files:

1. **`dmp.md`**  -  the plan in Markdown, section-headed, ready to paste into DMPTool or a proposal document.
2. **`dmp.docx`**  -  the same content formatted for direct submission (using the docx skill).

Include an appendix listing every funder-specific requirement your draft satisfies and any TBD items.

## Repository picker (domain → recommended repository)

| Data type | Recommended repository | Notes |
|-----------|------------------------|-------|
| Any published dataset, general | **Zenodo** | Free, DOI, CERN-backed, integrates with GitHub |
| General with light curation | **Figshare** | Free public tier; institutional tiers exist |
| Curated ecology/evolution/biology | **Dryad** | ~$120 deposit fee; strong curation |
| Genomic sequences | **NCBI GenBank / SRA / GEO** | Discipline-standard for sequencing |
| Human genomic (controlled access) | **dbGaP** (US) / **EGA** (EU) | Requires DAC approval |
| Neuroimaging | **OpenNeuro** | BIDS-compliant |
| Proteomics | **PRIDE** | ProteomeXchange member |
| Social science / survey / longitudinal | **ICPSR**, **UK Data Service**, **Qualitative Data Repository** | Long-standing, curated |
| Climate / earth science | **NASA EOSDIS**, **PANGAEA** | Discipline-standard |
| Materials science | **NOMAD**, **Materials Project** |  |
| Code | **GitHub** + **Zenodo DOI mirror** | GitHub for dev, Zenodo for citable version |
| Any (institutional) | **Institutional Repository** | Belt-and-suspenders alongside a domain repository |

Search re3data.org for others: <https://www.re3data.org/>

## FAIR quick-check

Before submission, walk each dataset through the four FAIR principles:

- **F**indable  -  DOI assigned; rich metadata; indexed by search engines and DataCite.
- **A**ccessible  -  retrievable via a standard protocol (HTTPS); metadata accessible even if data are under controlled access.
- **I**nteroperable  -  non-proprietary formats; community metadata standards; controlled vocabularies with resolvable identifiers.
- **R**eusable  -  clear license; provenance documented; community-relevant standards followed; enough context for a stranger to reuse it.

Use the **FAIR Cookbook** (<https://faircookbook.elixir-europe.org/>) and **F-UJI Automated FAIR assessment** (<https://www.f-uji.net/>) for a rigorous check.

## Standard tools

- **DMPTool** (<https://dmptool.org/>)  -  free, institutional accounts, pre-loaded funder templates
- **Argos** (<https://argos.openaire.eu/>)  -  European alternative, integrated with OpenAIRE and Zenodo
- **DMP Roadmap** (<https://github.com/DMPRoadmap>)  -  open-source DMPTool engine
- **maDMP** (machine-actionable DMP, RDA standard)  -  the future direction; DMPs as structured JSON, not just prose

## Tone and delivery

- Push users toward FAIR  -  even for controlled-access data  -  as the modern default.
- Explicitly recommend the domain-appropriate repository, not just Zenodo, when one exists (e.g., "sequencing data belongs in SRA, not Zenodo").
- If a funder mandate has an approaching deadline (e.g., NIH DMSP for all applications since Jan 2023), state it plainly.
- Frame the DMP as a *research asset*, not paperwork. A good DMP reduces the "where did we put that file?" tax on every project meeting for the next five years.
