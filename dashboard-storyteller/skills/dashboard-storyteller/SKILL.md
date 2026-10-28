---
name: dashboard-storyteller
description: Turn a chart, table, dashboard tile, or dataset into a plain-English narrative that surfaces the "so what" for a stakeholder audience. Produces executive-ready one-paragraph summaries, three-bullet talking points, or full narrative briefs from a screenshot, CSV, or description. Trigger this skill whenever the user mentions dashboard narrative, executive summary of a chart, "what does this chart show," "explain this table to my boss," "write up these numbers," turning data into a story, or preparing talking points from a report. Especially useful for institutional-research briefings, board reports, KPI dashboards, and any weekly/monthly metrics review where the audience won't read the underlying analysis themselves.
---

# Dashboard Storyteller

Author: Samah Alshrief · https://github.com/Samah989

Translate a chart, table, or dashboard tile into a written narrative that a busy stakeholder can absorb in 30 seconds. The default reader is an executive, board member, or department head  -  not a data analyst.

## When to use

- "Write me a paragraph explaining this chart to the provost / dean / CFO / director"
- "What are the three things a reviewer should take away from this table?"
- "Turn this dashboard tile into a slide caption"
- "I need talking points for tomorrow's board meeting from this Tableau/Power BI/Looker view"
- Weekly/monthly/quarterly metrics packages where an executive summary must precede the data

## Guiding principles

**Lead with the "so what," not the data.** Bad: "Enrollment in Fall 2024 was 6,214, down from 6,489 in Fall 2023." Good: "Fall enrollment fell 4% year-over-year  -  the third consecutive annual decline  -  driven entirely by out-of-state undergraduates."

**Compare, don't just report.** A number in isolation is a fact. A number vs. a benchmark, a target, a prior period, or a peer is a story. Always attach the comparison in the first sentence.

**Attribute the drivers.** If the aggregate moved, which segment(s) drove it? "Total spend rose 8%, but 70% of that increase came from three vendors in professional services."

**Name the uncertainty.** "Preliminary figures  -  subject to revision after the census freeze on Oct 15" is honest and preserves credibility.

## Workflow

### 1. Understand the input
Ask (or infer from what's given):
- What is the chart or table measuring? Include units.
- Time frame or snapshot date.
- The intended reader (executive, board, faculty, public).
- The desired output length: caption (1-2 sentences), summary (1 paragraph), talking points (3 bullets), brief (3-5 paragraphs).
- Any comparison targets available (prior period, budget, peer benchmark).

### 2. Extract the key facts
- What is the headline number?
- What direction and magnitude of change vs. the natural comparator?
- Which segment(s) explain most of the change?
- Any anomaly, outlier, or definitional caveat the reader needs?

### 3. Draft in one of these structures

**Caption (Twitter length)**
> [Metric] [direction] [magnitude] [comparison]. [Driver in one clause].
>
> Example: "Six-year graduation rate rose 1.8 pp to 71.4%, its highest ever, driven by gains among Pell-eligible undergraduates."

**Executive paragraph**
> Sentence 1: the headline and its comparator.
> Sentence 2: the primary driver.
> Sentence 3: the secondary driver or a caveat.
> Sentence 4: the implication or the next-look-at.
>
> Example: "First-year retention slipped to 84.1% in Fall 2024, down 1.2 pp from Fall 2023 and 0.6 pp below our five-year average. The decline is concentrated in first-generation students (-3.4 pp), while continuing-generation retention held steady. Housing-status data (not yet complete) may shed further light. Recommend a mid-year cohort check-in with Student Success Advisors."

**Three-bullet talking points**
> - Headline in a single sentence with the comparator.
> - Most surprising finding  -  what would the reader not expect?
> - What's the action or the next question this raises?

**Brief (3-5 paragraphs)**
> ¶1  -  Headline paragraph as above.
> ¶2  -  What's working / where the gains are.
> ¶3  -  What's not working / where risk is concentrated.
> ¶4  -  Data caveats and what a follow-up look would clarify.
> ¶5  -  Recommendation, if warranted.

### 4. Sanity-check
Before delivering, ask:
- Would a reader who never sees the chart still understand the finding?
- Am I comparing on a like-for-like basis? (Same denominator, same definition, same period boundary)
- Have I stated the uncertainty?
- Have I resisted the temptation to editorialize past what the data supports?

## Common institutional-research (IR) narratives

Reusable framings for the metrics that come up every week:

- **Enrollment**: total headcount, net change, driver segments (in/out-of-state, program area, class year), yield rate on admits.
- **Retention**: 1-yr → 2-yr → to-graduation transitions; sub-group differences (Pell, race/ethnicity, first-gen, in/out-of-state).
- **Graduation rate**: cohort year, exclusion notes (part-time, transfers), gap analyses (Pell vs non-Pell, gender, race).
- **Financial**: E&G revenue vs. expense trends, discount rate (institutional grants / gross tuition), reserves ratio.
- **Sponsored research**: awards volume vs. proposals volume vs. F&A recovery.
- **Diversity metrics**: student, faculty, staff composition trends; hiring vs. attrition breakdown.

## Anti-patterns to avoid

- Restating the y-axis in words.
- "As you can see"  -  the reader can't see; you're writing so they don't have to.
- Comparing to a cherry-picked base year that flatters the metric.
- Averages that hide a bimodal distribution.
- "Increased significantly" without stating whether "significant" means statistically significant or substantively meaningful.
- Percentage-of-a-percentage confusion ("retention rose 5%"  -  meaning 5 percentage points, or 5% relative? state which).

## Output-format conventions

- Round for readers: percentages to one decimal, dollars to nearest hundred or thousand depending on scale.
- Percentage-point differences use "pp" (`+1.8 pp`); relative changes use `%` (`+3.1%`).
- Dollar figures should carry the year for real-terms comparisons.
- Use "n =" to give the reader the sample size when one is relevant.

## Tone

- Confident but honest. If the data don't support a strong claim, don't smuggle one in.
- Neutral, not editorial. IR narratives inform decisions; they don't argue for them.
- Short. Every kept word earns its space.
- Reader-first: what does *this specific reader* need to know?
