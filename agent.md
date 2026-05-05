# Agent guide — offering-templates

Instructions for an AI agent creating or editing offerings in this repository.

## What this repo is

A collection of `.html` files, one per consulting offering. The [Offering Catalog](https://adrian-gruenther.de/exp/offering-catalog/) reads from this repo every 5 minutes and indexes all `.html` files automatically.

## How to create a new offering

1. Choose a slug: lowercase kebab-case, descriptive, unique. Example: `api-security-review`.
2. Create `<slug>.html` using the template below.
3. Commit directly to `main`. The catalog will pick it up within 5 minutes.

## Minimal template

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>TITLE</title>
  <meta name="slug"        content="SLUG" />
  <meta name="title"       content="TITLE" />
  <meta name="symbol"      content="EMOJI" />
  <meta name="lifecycle"   content="LIFECYCLE" />
  <meta name="summary"     content="SUMMARY — one sentence, max 200 chars." />
  <meta name="tags"        content="tag-one, tag-two" />
  <meta name="industries"  content="Industry A, Industry B" />
  <meta name="problems"    content="problem one, problem two" />
  <meta name="profiles"    content="CTO, Head of Engineering" />
  <meta name="methods"     content="Method A, Method B" />
  <meta name="technologies" content="Tool A, Tool B" />
  <meta name="next-offerings" content="" />
  <meta name="value-inputs"  content="employees:number:Number of Employees" />
  <meta name="value-formula" content="employees * 5000" />
  <meta name="value-label"   content="Estimated Annual Value (€)" />
  <meta name="contacts"      content="" />
  <meta name="opportunity-ids" content="" />
  <meta name="references"    content="" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <h2>What we do</h2>
  <p>PARAGRAPH</p>

  <h2>Approach</h2>
  <ol>
    <li><strong>Phase 1:</strong> Description.</li>
    <li><strong>Phase 2:</strong> Description.</li>
  </ol>

  <h2>Deliverables</h2>
  <ul>
    <li>Deliverable one</li>
    <li>Deliverable two</li>
  </ul>

  <h2>Typical engagement</h2>
  <p>X weeks · Y consultants · Fixed-fee / T&amp;M</p>
</body>
</html>
```

## Lifecycle values

Pick exactly one:

- `strategy` — upfront strategic work (assessments, roadmaps)
- `conceptual` — architecture and design
- `implementation` — build and delivery programmes
- `operation` — run, sustain, manage
- `continuous-improvement` — ongoing optimisation loops
- `change-management` — people, adoption, training

## Journey links (`next-offerings`)

Cross-references drive the knowledge graph and the "what comes next" sidebar on each offering page. Rules:
- Only reference slugs that **exist** in this repo. Unknown slugs are silently skipped.
- A typical offering references 0–3 successors.
- Avoid cycles.

## Value calculator

Three fields work together to produce the calculator widget on the offering detail page:

```
value-inputs:  fieldId:number:Human Label, fieldId2:number:Human Label2
value-formula: fieldId * fieldId2 * 500
value-label:   Result label shown to user (€)
```

- `fieldId` must be a valid JS identifier (no spaces, no hyphens).
- `value-formula` is evaluated as JavaScript; use only `+`, `-`, `*`, `/`, and the field IDs.
- Omit all three fields if no calculator is needed.

## Mermaid diagrams

Add inside `<body>` wherever it fits:

```html
<div class="mermaid">
  graph LR
    A[Discover] --> B[Design]
    B --> C[Deliver]
</div>
```

Keep it to one diagram per offering. `graph LR` (left-right) and `graph TD` (top-down) work best.

## Local preview

Open any `.html` file directly in a browser — no build step or server needed. The file includes `<link rel="stylesheet" href="style.css" />` which loads `style.css` from the same directory. It provides:

- Clean typography and spacing
- Styled headings, lists, tables, code blocks, and blockquotes
- `.mermaid` div rendered as a readable monospace block (the catalog app renders it as a real diagram; locally it shows the raw syntax)
- `.badge` and `.tag` chip classes for lifecycle labels

The stylesheet is cosmetic only — the catalog ignores it and parses only the `<meta>` tags and body HTML.

## What NOT to do

- Do not create non-`.html` files expecting them to be indexed — only `.html` is picked up.
- Do not use `prior-offerings` — that field is parsed but has no effect in the current catalog version; use `next-offerings` on the preceding offering instead.
- Do not put JavaScript or `<style>` blocks in the body — they will render as-is.
- Do not make the `slug` meta differ from the filename — they must match exactly.
- Do not remove the `<link rel="stylesheet" href="style.css" />` tag — it is needed for local preview.
