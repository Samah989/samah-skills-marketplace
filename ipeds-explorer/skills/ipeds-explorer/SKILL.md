---
name: ipeds-explorer
description: Answer higher-education institutional research questions using IPEDS data via the Urban Institute Education Data Portal  -  peer comparisons, enrollment trends, financial ratios, graduation rates, faculty size, tuition and net price, Pell-share, sector benchmarks  -  without touching a notebook. Trigger this skill whenever the user mentions IPEDS, institutional research, higher-ed benchmarking, peer group comparison, Carnegie classification, sector analysis, enrollment trend, graduation rate, retention rate, net price, tuition, endowment, or asks any question about a US postsecondary institution's characteristics or performance. Also trigger for phrases like "how does <university> compare to its peers," "what's the average net price at 4-year publics," "who are the peers of <university>," or "show me <metric> by state."
---

# IPEDS Explorer

Author: Samah Alshrief · https://github.com/Samah989

Answer higher-education institutional-research questions quickly by hitting the Urban Institute Education Data Portal (a free REST wrapper over IPEDS) directly. No notebook required  -  the skill fetches, tidies, and summarizes in one turn.

## When to use

- "How does <institution> compare to its peers on <metric>?"
- "What's the average <net price / graduation rate / Pell share / student-faculty ratio> at <sector>?"
- "Who are the Carnegie R1 peers of <institution>?"
- "Show me <metric> by state / by sector / over time"
- Any question naming IPEDS, UNITID, Carnegie classification, sector, control (public/private/for-profit), Pell, IPEDS survey names (HD, IC, EF, C, GR, F, SFA, SFAV)

## Guiding principles

**IPEDS is authoritative but slow.** New survey collections lag by 12-18 months. Latest cycles at any given time: HD/IC (fall directory) is usually current-year; Finance and SFA lag by 1-2 years; Graduation Rates lag by a full 6-year cohort cycle (2018 cohort's 6-year rate published in 2025).

**Every answer needs a UNITID + a year.** Never quote an "average net price" without stating the year, the sector, and the number of institutions in the sample. Report exclusions (e.g., "excluding institutions with <200 FTE" or "excluding for-profits").

**Peers are political.** IPEDS Data Feedback Reports use a system-defined peer set; institutions curate their own peer lists for benchmarking. When a user asks for "peers," clarify whether they want the DFR-default peers, self-defined peers, or a data-driven peer group (typically matched on Carnegie + control + enrollment + selectivity).

## Data source

Urban Institute Education Data Portal  -  a free, no-key REST API over IPEDS.

- Base: `https://educationdata.urban.org/api/v1/`
- Pattern: `/college-university/ipeds/{topic}/{year}/[nested]/?filters`
- Docs: <https://educationdata.urban.org/documentation/>
- Pagination: 10,000 rows/page, follow the `next` field.

The sister repo `Samah989/ipeds-institutional-analysis` has a reusable Python client (`src/data_pull.py`) with parquet caching  -  reuse it when this skill needs more than a one-shot answer.

## Common topics and columns

| Topic | Grain | Key columns |
|-------|-------|-------------|
| `directory` | inst-year | `unitid`, `inst_name`, `sector`, `control`, `state_abbr`, `fips`, `carnegie_size_setting` |
| `institutional-characteristics` | inst-year | `hbcu`, `hsi`, `carnegie_size_setting`, `urban_centric_locale` |
| `finance` | inst-year | `total_expenses`, `instruction_exp`, `endowment_end_of_year`, `state_govt_appropriations`, `enrollment_fte` |
| `sfa-all-undergraduates` | inst-year | `net_price`, `pct_pell`, `avg_grant_amt` |
| `grad-rates` | inst-cohort | `grad_rate_150_p` (6-yr rate for 4-yr institutions), `cohort_year` |
| `grad-rates-pell` | inst-cohort × pell status | Pell-disaggregated grad rates (2015+) |
| `fall-enrollment/race` | inst-year × race code | `enrollment_fall`, race codes 1-9 |
| `admissions-enrollment` | inst-year | `applicants`, `admissions`, `enrolled_full_time`, `admit_rate`, `sat_75`, `act_75` |

Sector codes (memorize these  -  every filter uses them):
- 1 = Public 4-yr+ · 2 = Private nonprofit 4-yr+ · 3 = Private for-profit 4-yr+
- 4 = Public 2-yr · 5 = Private nonprofit 2-yr · 6 = Private for-profit 2-yr
- Sub-2-year sectors (7, 8, 9)  -  mostly for-profit, small enrollment.

## Workflow

### 1. Parse the question
Extract: which institution(s), which metric(s), which year(s), which sector filter, whether comparison is peer-based or sector-wide.

### 2. Look up the target institution's UNITID
If the user says "Seton Hall University," resolve it to `189097` via `directory` with a `fips` + `inst_name` search. Confirm with the user before drilling deeper.

### 3. Pull the minimum data
Prefer one narrow API call over ten broad ones. If the question is "Seton Hall's 6-yr grad rate for 2018 cohort," fetch just `grad-rates` for that unitid + year, don't pull the national panel.

### 4. Frame the answer
Every quantitative answer includes:
- The exact number
- The unit (percent, dollars, count, FTE-normalized)
- The year and IPEDS survey
- The comparison group (single institution, peers, sector, national)
- The N in the comparison group
- A one-sentence caveat when the number is more nuanced than it looks

### 5. Peer-group construction (when asked)
Default recipe if the user doesn't specify:
1. Same Carnegie basic classification.
2. Same control (public/private-nonprofit/private-for-profit).
3. Within ±30% enrollment (FTE).
4. Same broad selectivity band (based on admit rate or SAT 75th).
Return the peer list with unitids so the user can inspect it.

### 6. Output
For a single answer: one paragraph with the number, comparison, and caveat.
For a set of comparisons: a Markdown table sorted by the metric of interest, plus a two-sentence interpretation.
For an "over time" question: a table with year rows, plus a note about survey lags.

## Standard caveats to attach

- **Financial data**: use real (inflation-adjusted) dollars for cross-year comparison. IPEDS reports nominal.
- **Graduation rates**: the standard IPEDS rate covers first-time full-time bachelor's-seeking students only  -  a shrinking slice of the modern student body. Mention Outcome Measures (OM) as the more inclusive alternative when relevant.
- **Enrollment**: 12-month unduplicated (IPEDS E12) vs fall snapshot (IPEDS EF) differ meaningfully at community colleges. Say which you're citing.
- **For-profit sector**: quality varies widely; institution-level context matters more than sector averages.

## Related IR data sources worth naming

- **College Scorecard** (US Dept of Education)  -  includes post-graduation earnings, unavailable in IPEDS.
- **NSF HERD**  -  R&D expenditures at research universities.
- **NSF SED**  -  doctorate awards by field.
- **AAUP Faculty Compensation Survey**  -  faculty salaries by rank.
- **CUPA-HR**  -  administrative and professional salaries.
- **Common Data Set**  -  self-reported by institutions; complements IPEDS on admissions detail.

## Tone

- Numbers with context. "Net price at Seton Hall in 2022 was $32,120  -  about $4,500 above the average for private nonprofit 4-year institutions in NJ (N=15)."
- Flag when a user's question would be better answered by a different dataset (Scorecard for earnings, HERD for R&D expenditures).
- Push back gently when a comparison is unfair  -  comparing an R1 to a regional teaching college on graduation rate hides more than it reveals.
