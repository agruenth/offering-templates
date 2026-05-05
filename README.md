# Offering Templates

This repository holds the service offerings displayed in the [Offering Catalog](https://adrian-gruenther.de/exp/offering-catalog/). Each offering is a single `.html` file. The catalog polls this repo every 5 minutes and re-indexes any changes automatically.

## File naming

One file per offering. The filename (without `.html`) becomes the slug used in URLs and cross-references:

```
digital-strategy-assessment.html   →  slug: digital-strategy-assessment
platform-engineering-setup.html    →  slug: platform-engineering-setup
```

Use lowercase kebab-case only. No spaces, no underscores.

## File structure

Each file is a standard HTML document. All metadata lives in `<meta>` tags in the `<head>`; the body is the rendered content.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Your Offering Title</title>

  <!-- Required -->
  <meta name="slug"      content="your-offering-slug" />
  <meta name="title"     content="Your Offering Title" />
  <meta name="lifecycle" content="strategy" />
  <meta name="summary"   content="One-sentence description shown on cards and in search." />

  <!-- Recommended -->
  <meta name="symbol"       content="🧭" />
  <meta name="tags"         content="tag-one, tag-two" />
  <meta name="industries"   content="Financial Services, Healthcare" />
  <meta name="problems"     content="problem one, problem two" />
  <meta name="profiles"     content="CTO, Head of Engineering" />
  <meta name="methods"      content="Wardley Mapping, Team Topologies" />
  <meta name="technologies" content="AWS, Kubernetes" />

  <!-- Journey links (comma-separated slugs that must exist in this repo) -->
  <meta name="next-offerings"  content="slug-a, slug-b" />

  <!-- Value calculator (optional) -->
  <meta name="value-inputs"  content="fieldId:type:Label, fieldId2:type:Label2" />
  <meta name="value-formula" content="fieldId * fieldId2 * 1000" />
  <meta name="value-label"   content="Estimated Annual Saving (€)" />

  <!-- Local preview stylesheet (ignored by the catalog) -->
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <h2>What we do</h2>
  <p>...</p>

  <h2>Approach</h2>
  <ol>
    <li><strong>Phase name:</strong> Description.</li>
  </ol>

  <h2>Deliverables</h2>
  <ul>
    <li>Deliverable one</li>
  </ul>

  <h2>Typical engagement</h2>
  <p>Duration · Team size · Commercial model</p>

  <!-- Optional Mermaid diagram -->
  <div class="mermaid">
    graph LR
      A[Start] --> B[End]
  </div>
</body>
</html>
```

## Metadata reference

| Field | Required | Description |
|---|---|---|
| `slug` | Yes | URL-safe identifier. Must match the filename. |
| `title` | Yes | Display name shown on cards and the detail page. |
| `lifecycle` | Yes | See lifecycle values below. |
| `summary` | Yes | One sentence. Shown on the card and in search results. |
| `symbol` | No | Single emoji shown on the card. Defaults to 📦. |
| `tags` | No | Comma-separated free-form labels. Used for filtering and similarity. |
| `industries` | No | Comma-separated target industries. |
| `problems` | No | Comma-separated problem statements this offering addresses. |
| `profiles` | No | Comma-separated buyer/user personas (e.g. "CTO, VP Engineering"). |
| `methods` | No | Comma-separated methodologies used. |
| `technologies` | No | Comma-separated tools and platforms involved. |
| `next-offerings` | No | Comma-separated slugs of offerings that naturally follow this one. Used to draw the journey graph. Only reference slugs that exist in this repo. |
| `value-inputs` | No | Comma-separated calculator inputs. Each in the format `id:type:Label`. Types: `number`. |
| `value-formula` | No | JavaScript expression using the input IDs to compute a value. |
| `value-label` | No | Label for the computed value shown in the calculator. |
| `contacts` | No | Comma-separated MHP contact persons. Add role in parentheses: `"Jane Doe (Partner), Max Mustermann (Senior Manager)"`. |
| `opportunity-ids` | No | Comma-separated CRM/Salesforce opportunity IDs: `"OPP-12345, OPP-67890"`. |
| `references` | No | Comma-separated links in the format `Label\|URL`: `"Confluence Page\|https://..., Proposal\|https://..."`. |

## Lifecycle values

The `lifecycle` field controls which column an offering appears in on the grid view:

| Value | Meaning |
|---|---|
| `strategy` | Upfront strategic work |
| `conceptual` | Architecture and design |
| `implementation` | Build and delivery |
| `operation` | Run and sustain |
| `continuous-improvement` | Ongoing optimisation |
| `change-management` | People and adoption |

## Local preview

Open any `.html` file directly in a browser — no build step or server needed. Each file includes:

```html
<link rel="stylesheet" href="style.css" />
```

`style.css` lives in the repo root alongside the offering files. It provides clean typography, styled headings, lists, tables, code blocks, and badge/tag chip classes. The `.mermaid` div is shown as readable monospace text locally; the catalog renders it as a live diagram.

The stylesheet is cosmetic only — the catalog ignores it and reads only the `<meta>` tags and body HTML.

## Mermaid diagrams

Add a `<div class="mermaid">` block anywhere in the body to render a diagram. Standard [Mermaid](https://mermaid.js.org/) syntax is supported. Keep diagrams simple — `graph LR` and `graph TD` flowcharts work best.

## Sync behaviour

- The catalog checks for new commits every **5 minutes**.
- Only `.html` files are indexed; other files are ignored.
- `next-offerings` references to slugs that don't exist in the repo are silently skipped (no error).
- Deleting a file removes the offering from the catalog on the next sync cycle.
