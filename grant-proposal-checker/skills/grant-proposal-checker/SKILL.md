---
name: grant-proposal-checker
description: Compliance and completeness pass on a research grant proposal  -  verifies page limits, biosketch format, DMP/DMSP presence, budget-justification structure, broader-impacts section (NSF), significance/innovation/approach subheads (NIH), required attachments, references section formatting, and font/margin rules. Supports NSF (PAPPG), NIH (SF424 R&R + G.400 series), ERC, Horizon Europe, Wellcome, and DOE/DOD proposals. Trigger this skill whenever the user says "review my proposal," "is my grant ready to submit," "compliance check," "PAPPG compliance," "NIH biosketch," "does this meet the page limit," "check my broader impacts section," "budget justification review," or uploads a draft proposal document. Also trigger when the user mentions specific mechanisms (R01, R21, K99, CAREER, RUI, MRI, GRFP, ERC Starting, Horizon Europe RIA/IA).
---

# Grant Proposal Checker

Author: Samah Alshrief · https://github.com/Samah989

Compliance-and-completeness review of a research grant proposal against the funder's rules. This skill catches the mistakes that get proposals returned without review  -  page-limit violations, missing sections, wrong biosketch format, non-compliant fonts  -  before submission.

## When to use

- User uploads or references a draft grant proposal
- "Review my proposal," "is this compliant," "will this be returned without review"
- "PAPPG check" (NSF), "SF424 check" (NIH), "ERC check," "Horizon Europe check"
- Specific mechanisms: R01, R21, K99/R00, F31, F32, CAREER, RUI, MRI, MSPI, HDR, GRFP, ERC Starting/Consolidator/Advanced, MSCA, Horizon Europe RIA/IA/CSA
- Any question about biosketch format, DMP requirement, page limits, fonts, margins

## Guiding principles

**This skill checks compliance, not scientific merit.** Reviewers judge merit; program officers reject non-compliant proposals before review. Both matter. This skill's job is the second.

**Cite the specific rule.** "Your biosketch is too long" is a complaint. "Your NIH biosketch exceeds the 5-page limit specified in NOT-OD-24-013 (effective Jan 2025)" is actionable. Every finding must cite the funder document and section.

**Funder rules change.** Always confirm the version of the funder's guide against the current effective date before rendering a strict verdict. When uncertain, phrase findings as "based on <current known version>, this appears to..." and encourage the user to double-check current guidance.

## Workflow

### 1. Intake
Confirm:
1. **Funder + mechanism** (e.g., NSF CAREER, NIH R01, ERC Starting Grant, Horizon Europe HORIZON-CL5).
2. **Program solicitation number** if applicable (NSF has program-specific solicitations layered over PAPPG; NIH has FOA-specific rules).
3. **Submission deadline** (rules effective on submission date, not draft date).
4. **What documents are ready** (project narrative, biosketches, budget, budget justification, DMP, references, letters of support, current & pending, facilities, data management plan).

### 2. Load the funder rulebook

| Funder | Master document | Key layered docs |
|--------|-----------------|------------------|
| **NSF** | PAPPG (Proposal & Award Policies & Procedures Guide)  -  current chapter II | Program solicitation; DCL; broader-impacts guidance |
| **NIH** | SF424 (R&R) Application Guide + G.400 series | FOA (Funding Opportunity); NOT- policy notices (esp. NOT-OD-24-013 biosketch, DMSP NOT-OD-21-013) |
| **ERC** | ERC Work Programme + Information for Applicants | Panel-specific guidance |
| **Horizon Europe** | HE Programme Guide + General Annexes | Topic conditions in the work programme |
| **Wellcome** | Scheme-specific guidance | Cross-cutting policies on OA, DMP, EDI |
| **DOE** | Merit Review Plan + specific FOA | Office-specific supplements |
| **DOD** | BAA + FY-specific guidance | Agency (DARPA, ONR, ARO) supplements |

### 3. Run the compliance checks

Walk through each applicable check. Report per finding: **document + page/section, rule violated (with citation), severity, fix**.

#### Universal checks
- **Font**: NSF requires Arial, Helvetica, Palatino, or Georgia ≥ 10 pt (11 pt strongly preferred). NIH allows Arial, Georgia, Helvetica, Palatino Linotype at ≥ 11 pt (small text ≥ 9 pt allowed for figures/tables).
- **Margins**: NSF 1"; NIH 0.5" all sides.
- **Line spacing**: NSF single or greater; NIH no line spacing rule, but density often flags reviewers.
- **Page limits per section**: enforce strictly. See the mechanism table below.

#### Mechanism-specific page limits (illustrative  -  always verify against current solicitation)

**NSF**
- Project Description: 15 pages (some solicitations shorter, e.g., CAREER 15, RUI 15, MRI 10)
- Broader Impacts must have its own subhead within the Project Description
- References Cited: no limit but format required
- Biographical Sketch: 3 pages, using approved formats (SciENcv NSF-approved is the default since Oct 2023)
- Current & Pending Support: use SciENcv or NSF fillable PDF
- Data Management & Sharing Plan (DMSP): 2 pages max
- Budget Justification: 5 pages
- Facilities, Equipment, and Other Resources: no limit but must be substantive

**NIH**
- Research Strategy: R01 12 pages; R21 6 pages; R03 6 pages; F awards 6 pages; K awards vary
- Structure required: Significance / Innovation / Approach subheads (Approach must include Rigor of Prior Research + Sex-as-a-Biological-Variable considerations)
- Specific Aims: 1 page
- Biographical Sketch: 5 pages max, using the format effective Jan 2025 (personal statement + up to 4 sections including scholastic performance for early-stage investigators)
- Data Management and Sharing Plan: 2 pages max (per NOT-OD-21-013)
- References: no limit, but PubMed-style formatting required
- Budget Justification: no length limit but must justify each budget category
- Human Subjects sections if applicable
- Vertebrate Animals section if applicable
- Authentication of Key Biological/Chemical Resources (for basic-science apps): 1 page

**ERC (Starting/Consolidator/Advanced)**
- Part B1 (extended synopsis): 5 pages
- Part B2 (scientific proposal): 15 pages
- CV: 2 pages
- Track record (early-mid-career PIs): 2 pages
- Research environment: 1 page

**Horizon Europe RIA/IA**
- Part B page limits vary by topic; typically 45 pages for RIA/IA main technical section
- Ethics self-assessment, gender dimension, open-science practices, data-management approach required

#### Section-completeness checks

For NSF proposals, verify presence of: Cover Sheet, Project Summary (with Overview / Intellectual Merit / Broader Impacts labels), Project Description, References Cited, Biographical Sketches (one per Senior/Key), Budget + Justification, Current & Pending Support (one per person), Facilities, Data Management & Sharing Plan, any solicitation-specific attachments (mentoring plan for postdocs, RCR plan, etc.).

For NIH proposals: Cover Letter (optional but recommended), Project Summary/Abstract, Project Narrative (3 sentences, plain language), Bibliography & References Cited, Facilities & Other Resources, Equipment, Biosketches (one per Senior/Key), Budget + Justification, Specific Aims, Research Strategy, Vertebrate Animals (if applicable), Human Subjects sections (if applicable), Recruitment/Retention plan, PHS Inclusion Enrollment Report, Data Management and Sharing Plan, Authentication of Key Resources, letters of support, resource-sharing plans, multi-PI leadership plan (if multi-PI).

### 4. Output the report

```markdown
# Grant compliance review  -  <mechanism>
_Funder: <NSF/NIH/etc.>_ · _Solicitation: <ID>_ · _Deadline: <date>_ · _Reviewed against: <PAPPG version / NOT- number>_

## Verdict
- **Ready to submit**: yes / no / no-with-caveats
- **Return-without-review risk**: high / medium / low
- **Blocker findings**: <N>  ·  **High**: <N>  ·  **Recommendations**: <N>

## Blocker findings (must fix)

### 1. <Short title>
- **Rule**: <funder document + section>, e.g., "PAPPG Ch II.D.2.d"
- **Finding**: <specific violation, with document and page reference>
- **Fix**: <specific change>

## High-severity findings
## Recommendations (not required but strongly suggested)

## Sections verified present
- <bulleted list of every required section that was confirmed>

## Sections missing or unverified
- <bulleted list of anything the user hasn't provided>

## Notes on funder-guidance version
- <state the version you checked against + effective date>
```

### 5. Scientific-merit feedback (optional, if the user asks)

This skill's default is compliance-only. If the user explicitly requests scientific/narrative feedback, provide it but keep it clearly separated from the compliance findings. Merit feedback should touch on: clarity of the Specific Aims / research question, strength of the significance argument, methodological rigor and preliminary data, broader impacts (NSF) or public-health impact (NIH), fit to the program, and whether the budget matches the scope.

## Standard reference URLs

- NSF PAPPG (current): <https://www.nsf.gov/publications/pub_summ.jsp?ods_key=pappg>
- NIH Grants Policy Statement + How-to-Apply: <https://grants.nih.gov/grants/how-to-apply-application-guide.html>
- NIH biosketch formats + SciENcv: <https://grants.nih.gov/grants/forms/biosketch.htm>
- NIH DMSP guidance: <https://sharing.nih.gov/data-management-and-sharing-policy>
- ERC Work Programme: <https://erc.europa.eu/apply-grant>
- Horizon Europe reference documents: <https://ec.europa.eu/info/funding-tenders/opportunities/portal/screen/how-to-participate/reference-documents>
- SciENcv (NSF and NIH biosketch generator): <https://www.ncbi.nlm.nih.gov/sciencv/>

## Tone

- Direct on blockers. "This is 3 pages over the limit  -  cut before submission" beats "consider tightening."
- Cite the rule so the user can verify.
- Distinguish blocker (returned without review) from recommendation (weakens the proposal but wouldn't stop it).
- If the user's timeline is tight, prioritize the blocker list first; recommendations can wait.
- Recommend the office of sponsored research at the user's institution for institutional-signature items (Facilities & Other Resources standard language, indirect-cost rate, F&A negotiation, human-subjects assurance number).
