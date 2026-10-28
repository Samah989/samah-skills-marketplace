# samah-skills

A marketplace of eight skills by **Samah Alshrief** ([github.com/Samah989](https://github.com/Samah989)).

Each plugin is a self-contained skill that triggers automatically when its topic comes up.

## Plugins

| Plugin | Version | Category | What it does |
|--------|--------:|----------|--------------|
| [`website-accessibility`](./website-accessibility) | 1.0.0 | developer-tools | Audit websites for WCAG 2.2 compliance  -  automated axe-core scan + manual review, prioritized report with WCAG citations. |
| [`systematic-literature-review`](./systematic-literature-review) | 1.0.0 | research-methods | Guide a rigorous systematic or scoping review following PRISMA 2020  -  protocol, search-string design, deduplication, screening, extraction, risk-of-bias, PRISMA flow. |
| [`data-management-plan`](./data-management-plan) | 1.0.0 | research-methods | Generate a funder-compliant DMP (NSF, NIH, Wellcome, ERC, Horizon Europe, MRC, NSERC, ARC) with FAIR narrative, repository selection, and cost estimation. |
| [`reproducibility-audit`](./reproducibility-audit) | 1.0.0 | research-methods | Audit a research repo for computational reproducibility  -  env pinning, seeds, data provenance, notebook hygiene, containers, CI, README. Severity-ranked report with fixes. |
| [`ipeds-explorer`](./ipeds-explorer) | 1.0.0 | higher-education | Answer higher-ed IR questions using IPEDS via the Urban Institute Education Data Portal  -  peer comparisons, enrollment trends, graduation rates, net price. |
| [`openalex-bibliometrics`](./openalex-bibliometrics) | 1.0.0 | research-methods | Bibliometric analysis via OpenAlex  -  free open scholarly graph. Author profiles, institution profiles, citations, co-authorship, funders. |
| [`dashboard-storyteller`](./dashboard-storyteller) | 1.0.0 | communication | Turn a chart or table into a plain-English narrative for executives  -  captions, paragraphs, talking points, or full briefs. |
| [`grant-proposal-checker`](./grant-proposal-checker) | 1.0.0 | research-administration | Compliance and completeness pass on NSF/NIH/ERC/Horizon Europe/Wellcome/DOE/DOD grant proposals. |

## Install

```
/plugin marketplace add Samah989/samah-skills-marketplace
```

Install any (or all):

```
/plugin install website-accessibility@samah-skills
/plugin install systematic-literature-review@samah-skills
/plugin install data-management-plan@samah-skills
/plugin install reproducibility-audit@samah-skills
/plugin install ipeds-explorer@samah-skills
/plugin install openalex-bibliometrics@samah-skills
/plugin install dashboard-storyteller@samah-skills
/plugin install grant-proposal-checker@samah-skills
```

## Layout

```
samah-skills-marketplace/
├── .claude-plugin/marketplace.json      Catalog of all plugins
├── <plugin-name>/                        One folder per plugin
│   ├── .claude-plugin/plugin.json        Plugin manifest
│   └── skills/<plugin-name>/
│       ├── SKILL.md
│       └── references/ (optional)
├── README.md
├── LICENSE
└── setup_instructions.md
```

## License

MIT  -  see [LICENSE](./LICENSE).
