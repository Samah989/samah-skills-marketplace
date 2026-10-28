# Setup

Eight plugins in this marketplace:

- `website-accessibility`
- `systematic-literature-review`
- `data-management-plan`
- `reproducibility-audit`
- `ipeds-explorer`
- `openalex-bibliometrics`
- `dashboard-storyteller`
- `grant-proposal-checker`

## Push the repo

First commit:

```bash
cd samah-skills-marketplace
git init
git branch -M main
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/Samah989/samah-skills-marketplace.git
git push -u origin main
```

Subsequent updates:

```bash
cd samah-skills-marketplace
git add .
git commit -m "Update"
git push
```

## Install

```
/plugin marketplace add Samah989/samah-skills-marketplace
/plugin install website-accessibility@samah-skills
/plugin install systematic-literature-review@samah-skills
/plugin install data-management-plan@samah-skills
/plugin install reproducibility-audit@samah-skills
/plugin install ipeds-explorer@samah-skills
/plugin install openalex-bibliometrics@samah-skills
/plugin install dashboard-storyteller@samah-skills
/plugin install grant-proposal-checker@samah-skills
```
