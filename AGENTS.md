# AGENTS.md

Guidance for AI coding agents (Claude, Cursor, Copilot, etc.) working with this repo. This is a static
data repository — 180 curated gradients, no build step, no dependencies.

## Files and what they're for

| File | Gradients | Use it for |
| --- | --- | --- |
| [`webgradients.css`](webgradients.css) | 180 (complete, canonical) | Ready-to-use CSS classes. Source of truth for the full set. |
| [`gradients.json`](gradients.json) | 180 (complete) | Structured metadata regenerated from `webgradients.css`: name, angle, color stops. Preferred source for programmatic use. |
| [`gradients-parsed.json`](gradients-parsed.json) | 174 | Older structured dataset (site-internal history). Missing the same 6 entries as before (blend-mode composites); its 2 name typos (`Arielles Smile`, `October Silenceiver`) have been fixed. Prefer `gradients.json` for the complete set. |

### Why `gradients.json` only had 11 entries until now

It was an abandoned 2019 prototype (see `git log -- gradients.json`) — the real data work happened in
`gradients-parsed.json` instead, which itself topped out at 174/180 because 6 gradients
(`Coup de Grace`, `Loon Crest`, `Sharp Glass`, `Chemic Aqua`, `Slick Carbon`, `Earl Gray`) use layered
`background-blend-mode` effects that don't reduce to a single `{deg, gradient}` pair, so the original
pipeline silently dropped them. `gradients.json` is now regenerated directly from `webgradients.css`
(the canonical, complete source) to close that gap:

- 174 entries: color-stop data reused from `gradients-parsed.json` (name-matched; the source file's 2 typos, `Arielles Smile` and `October Silenceiver`, have since been fixed in place there too).
- 5 entries (`Coup de Grace`, `Loon Crest`, `Sharp Glass`, `Chemic Aqua`, `Earl Gray`): no layer has real
  hex color stops — they're a flat base color under a translucent blend-mode sheen — so they're
  represented as a flat 2-stop gradient of that base color (not a fabricated second color).
- `Slick Carbon`: its primary layer does have real hex stops, extracted directly from the CSS.
- All 6 are grouped under `#E5E9EC` (the palette's only neutral/gray bucket), consistent with the 4
  other blend-mode composites already in the dataset (`Above Clouds`, `Raccoon Back`, `Elegance`,
  `Full Metal`), which are grouped the same way.

If regenerating `gradients.json` again, always derive from `webgradients.css`, not from
`gradients-parsed.json` — the CSS is the only file guaranteed to have all 180 and the correct names.

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
| `index` | string | 1-based position, zero-padded (`"001"`–`"180"`), matches the CSS comment number. |
| `deg` | number | CSS gradient angle in degrees (`linear-gradient(<deg>deg, ...)`). |
| `group` | string[] | One or more representative/dominant hex colors for the gradient. |
| `gradient` | object[] | Ordered color stops: `color` (hex) and `pos` (0–100, percent position). |

To reconstruct the CSS from JSON: `linear-gradient(${deg}deg, ${gradient.map(s => `${s.color} ${s.pos}%`).join(', ')})`.

See [`add_gradients.md`](add_gradients.md) for the original authoring guide (color groups, contribution format).

## Conventions for agents

- Don't regenerate or reformat `webgradients.css` — it's hand-curated and order-sensitive (index order matches the site).
- If asked to add a gradient, follow `add_gradients.md`, append to the end of all three sources you touch, and keep `index` in sync.
- This repo has no `nextjs`/build tooling of its own — [webgradients.com](https://webgradients.com) is a separate application that consumes this data.
