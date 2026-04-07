---
name: visualization
description: Use when users ask for charts, infographics, icon lookup, narrative text visualization, or AntV S2 cross-analysis table help.
---

# Visualization

Unified AntV visualization skill bundle. This folder groups chart generation, icon retrieval, infographic authoring, T8 narrative text visualization, and `@antv/s2` table guidance under one entry point.

## Route By Intent

- **Charts and maps**: Use `chart-visualization/` resources.
  - Read the matching reference in `chart-visualization/references/` before building args.
  - Run `node ./chart-visualization/scripts/generate.js '<payload_json>'`.
- **Icon search / SVG retrieval**: Use `icon-retrieval/`.
  - Run `node ./icon-retrieval/scripts/search.js '<search_query>' [topK]`.
- **Infographics**: Use `infographic-creator/SKILL.md` for the AntV Infographic DSL and HTML rendering workflow.
- **Narrative text visualization**: Use `narrative-text-visualization/SKILL.md` for T8 syntax, entity annotations, and rendering examples.
- **AntV S2 / pivot tables / cross tables**: Use `antv-s2-expert/SKILL.md` plus the references under `antv-s2-expert/references/`.

## Quick Rules

- Do not guess chart args; read the matching chart reference first.
- Use Node.js 18+ for the chart and icon scripts.
- For S2 questions, route by topic using the table in `antv-s2-expert/SKILL.md`.
- For infographic and T8 requests, preserve the user's language in generated content.

## Supporting Layout

- `chart-visualization/`: chart references and generator script
- `icon-retrieval/`: icon search script
- `infographic-creator/`: infographic instructions
- `narrative-text-visualization/`: T8 narrative text instructions
- `antv-s2-expert/`: S2 reference library
