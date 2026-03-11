# debugCSS

`debugCSS.css` is a single drop-in stylesheet for visually flagging malformed,
inaccessible, security-sensitive, or legacy markup on an existing page.

The file focuses on issues that are still useful to catch in current HTML:

- missing document language or title
- missing `alt` text and broken image sources
- empty buttons or links with no accessible name
- focusable content hidden with `aria-hidden`
- unlabeled `iframe`, `role="img"`, `role="dialog"`, and `role="slider"` patterns
- required ARIA widget states like `aria-checked`, `aria-selected`, and `aria-expanded`
- positive `tabindex`, `accesskey`, autoplay media, and radio inputs with no `name`
- `target="_blank"` links without `rel="noopener"` or `rel="noreferrer"`
- invalid list structure and empty headings
- deprecated elements and legacy presentational attributes
- missing table captions and fieldset legends when the browser supports `:has()`

## How To Use It

Load `debugCSS.css` into the page you want to inspect. The stylesheet adds:

- colored outlines around matching elements
- small inline message chips near each issue
- a fixed summary panel showing severity counts plus a11y / structure / legacy / security totals

Severity colors:

- Error: something is likely broken or inaccessible
- Warning: likely worth fixing soon
- Notice: legacy or context-dependent issue

## Modes And Filters

The stylesheet supports a few CSS-only control hooks on the root `html` element:

- `data-debugcss-compact`
  - hides most inline message chips until hover or focus
- `data-debugcss-mode="a11y structure legacy security"`
  - space-separated category filter
  - example: `data-debugcss-mode="a11y security"`
- `data-debugcss-level="error warning notice"`
  - space-separated severity filter
  - example: `data-debugcss-level="error warning"`

You can combine them:

```html
<html data-debugcss-compact data-debugcss-mode="a11y security" data-debugcss-level="error warning">
```

Document-level checks on the root element, such as missing `lang`, may remain
visible even when filters are active.

## Modern Browser Notes

The base rules work in current evergreen browsers.

Extra rules that rely on `:has()` are automatically enabled when the browser
supports that selector. Those checks cover things like:

- missing `<title>`, `<meta charset>`, or `<main>`
- labels that neither wrap a control nor point at one
- form controls with no obvious built-in label path
- `details` without `summary`
- `fieldset` without `legend`
- data tables without `caption`
- inline SVG images with no accessible name
- sections with no heading or accessible name
- videos with controls but no caption or subtitle track

## Escape Hatches

Use `data-debugcss-ignore` on an element to suppress debugCSS for that element
and its subtree.

Use `.debugcss-alpha` or `[data-debugcss-alpha]` to apply the embedded
checkerboard background when you want transparent images to sit on a visible
pattern.

The old standalone-image behavior is still present for `body > img:only-child`.

## Limits

This is still CSS. It can highlight a lot, but it cannot reliably detect
everything that requires DOM relationships, computed accessible names, or
scripted behavior. The rule set therefore prefers high-signal checks over
trying to guess every possible accessibility bug.

The summary panel is best treated as approximate. CSS counters cannot count
multiple separate rule hits on the exact same element with perfect accuracy,
so the totals are most useful as a quick signal rather than a strict audit
report.

## License

Copyrights for code authored by Yahoo! Inc. are licensed under the MIT License.
