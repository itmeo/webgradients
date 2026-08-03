# WebGradients

[![Stars](https://img.shields.io/github/stars/itmeo/webgradients?style=for-the-badge&color=635BFF)](https://github.com/itmeo/webgradients/stargazers)
[![License](https://img.shields.io/github/license/itmeo/webgradients?style=for-the-badge)](LICENSE)
[![Last commit](https://img.shields.io/github/last-commit/itmeo/webgradients?style=for-the-badge)](https://github.com/itmeo/webgradients/commits/master)

<a href="https://donate.itmeo.com">
  <img src="https://img.shields.io/badge/Donate-donate.itmeo.com-635BFF?style=for-the-badge&logo=heart&logoColor=white" alt="Donate" height="50">
</a>

If you like WebGradients, consider supporting the project — it helps keep the tools free, fast, and ad-free.

A curated collection of 180 splendid gradients made in `CSS3`, `.sketch`, `.PSD` and `Figma` formats — free for personal and commercial use.
[View all the gradients here »](https://webgradients.com)

Made by [Dima Braven](https://dimabraven.com) · [itmeo](https://itmeo.com)

## Machine-readable data

All 180 gradients are also available as structured JSON — useful for design tools, generators, or feeding gradient data to an LLM/agent:

- [`gradients.json`](gradients.json) — full dataset with per-stop color positions
- [`gradients-parsed.json`](gradients-parsed.json) — flattened variant with zero-padded index

```json
{
  "name": "Warm Flame",
  "index": "001",
  "deg": 45,
  "group": ["#F9AFAD"],
  "gradient": [
    { "color": "#ff9a9e", "pos": 0 },
    { "color": "#fad0c4", "pos": 100 }
  ]
}
```

`deg` is the CSS gradient angle, `group` is the dominant color(s), `gradient` is the ordered list of color stops (`pos` in %).

## Figma Plugin

Install the [WebGradients Figma plugin](https://www.figma.com/community/plugin/802147585857776440/webgradients) to use all 180 gradients directly inside Figma.

## How To Use

1. Download the file [`webgradients.css`](https://github.com/itmeo/webgradients/blob/master/webgradients.css).
2. Place the file in your project folder.
3. Link the file in the `<head>` of your document.

```html
<html>
  <head>
    <link href="webgradients.css" rel="stylesheet">
  </head>
  ...
```

## Browser Compatibility

Some gradients use the `background-blend-mode` CSS property. It is supported by the majority of modern browsers.
View full [compatibility list (view on Caniuse) »](http://caniuse.com/#search=background-blend-mode)

You can learn more about `background-blend-mode` [here (view on MDN) »](https://developer.mozilla.org/en-US/docs/Web/CSS/background-blend-mode)

## License

WebGradients is created under the [MIT](http://opensource.org/licenses/MIT) license.
