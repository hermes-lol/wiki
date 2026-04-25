# Wiki Schema

## Domain
经济社会运行的基本原理 —— 从经济视角出发，理解人类社会政治文化的运行规律。

## Conventions
- File names: lowercase, hyphens, no spaces (e.g., `money-circulation.md`)
- Every wiki page starts with YAML frontmatter (see below)
- Use `[[wikilinks]]` to link between pages (minimum 2 outbound links per page)
- When updating a page, always bump the `updated` date
- Every new page must be added to `index.md` under the correct section
- Every action must be appended to `log.md`

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
---
```

## Tag Taxonomy

### 核心维度
- **经济基础**: production, distribution, exchange, consumption, currency, capital, labor, market, trade
- **权力结构**: authority, governance, politics, state, law, institution, hierarchy, conflict
- **社会形态**: society, culture, norms, belief, identity, class, mobility, conflict, cooperation
- **运行机制**: cycle, flow, evolution, crisis, reform, revolution, equilibrium

### 关系标签
- **因果**: cause-effect, correlation, feedback-loop
- **比较**: comparison, analogy, contrast
- **演变**: emergence, transformation, collapse, succession

## Page Thresholds
- **Create a page** when an entity/concept appears in 2+ sources OR is central to one source
- **Add to existing page** when a source mentions something already covered
- **DON'T create a page** for passing mentions, minor details, or things outside the domain
- **Split a page** when it exceeds ~200 lines — break into sub-topics with cross-links
- **Archive a page** when its content is fully superseded — move to `_archive/`, remove from index

## Entity Pages
One page per notable entity. Include:
- Overview / what it is
- Key facts and dates
- Relationships to other entities ([[wikilinks]])
- Source references

## Concept Pages
One page per concept or topic. Include:
- Definition / explanation
- Current state of knowledge
- Open questions or debates
- Related concepts ([[wikilinks]])

## Comparison Pages
Side-by-side analyses. Include:
- What is being compared and why
- Dimensions of comparison (table format preferred)
- Verdict or synthesis
- Sources

## Update Policy
When new information conflicts with existing content:
1. Check the dates — newer sources generally supersede older ones
2. If genuinely contradictory, note both positions with dates and sources
3. Mark the contradiction in frontmatter: `contradictions: [page-name]`
4. Flag for user review in the lint report