# AGENTS.md

Guidance for AI coding agents (Claude, Cursor, Copilot, etc.) working with this repo. This is a static
data repository — 180 curated gradients, no build step, no dependencies.

## Files and what they're for

| File | Gradients | Use it for |
| --- | --- | --- |
| [`webgradients.css`](webgradients.css) | 180 (complete, canonical) | Ready-to-use CSS classes. Source of truth for the full set. |
| [`gradients-parsed.json`](gradients-parsed.json) | 174 | Structured metadata: name, angle, color stops. Best source for programmatic use. |
| [`gradients.json`](gradients.json) | 11 | Legacy/partial dataset kept for backwards compatibility. Do not treat as complete — use `gradients-parsed.json` instead. |

There is no PNG/Figma/Sketch/PSD data in this repo — those formats are distributed via
[webgradients.com](https://webgradients.com) and the [Figma plugin](https://www.figma.com/community/plugin/802147585857776440/webgradients),
not as files here.

## `webgradients.css` structure

One CSS class per gradient, in numeric order, each preceded by a `/*NNN Name*/` comment:

```css
/*001 Warm Flame*/
.warm_flame{
    background-image: linear-gradient(45deg, #ff9a9e 0%, #fad0c4 99%, #fad0c4 100%);
}
```

Class name = gradient name, lowercased, spaces replaced with `_` (e.g. `"Warm Flame"` → `.warm_flame`).
Every class name in the file matches `^[a-z0-9_]+$` — safe to parse with a simple regex.

## `gradients-parsed.json` / `gradients.json` structure

```json
{
  "name": "Warm Flame",
  "favorite": false,
  "index": "001",
  "deg": 45,
  "group": ["#F9AFAD"],
  "gradient": [
    { "color": "#ff9a9e", "pos": 0 },
    { "color": "#fad0c4", "pos": 99 },
    { "color": "#fad0c4", "pos": 100 }
  ]
}
```

| Field | Type | Meaning |
| --- | --- | --- |
| `name` | string | Gradient name, matches the CSS comment and (slugified) the CSS class. |
| `favorite` | boolean | Curation flag, not meaningful to consumers — always `false` in practice. |
| `index` | string/number | 1-based position. Zero-padded string (`"001"`) in `gradients-parsed.json`, plain number in `gradients.json`. |
| `deg` | number | CSS gradient angle in degrees (`linear-gradient(<deg>deg, ...)`). |
| `group` | string[] | One or more representative/dominant hex colors for the gradient. |
| `gradient` | object[] | Ordered color stops: `color` (hex) and `pos` (0–100, percent position). |

To reconstruct the CSS from JSON: `linear-gradient(${deg}deg, ${gradient.map(s => `${s.color} ${s.pos}%`).join(', ')})`.

See [`add_gradients.md`](add_gradients.md) for the original authoring guide (color groups, contribution format).

## Conventions for agents

- Don't regenerate or reformat `webgradients.css` — it's hand-curated and order-sensitive (index order matches the site).
- If asked to add a gradient, follow `add_gradients.md`, append to the end of all three sources you touch, and keep `index` in sync.
- This repo has no `nextjs`/build tooling of its own — [webgradients.com](https://webgradients.com) is a separate application that consumes this data.
