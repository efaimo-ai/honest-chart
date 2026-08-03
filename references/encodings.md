# Encodings: the chart for the data's shape

Pick the encoding from what the data IS, not from what looks impressive. The
wrong encoding misleads even when every number is right.

| the data is about | use | not |
|---|---|---|
| comparing amounts across categories | bar (horizontal if labels are long) | pie, 3D bar |
| a value over time or an ordered axis | line, or bar for few discrete points | smoothed spline that invents points |
| parts of one whole that sum to 100% | stacked bar, or a pie with <= 5 slices | a pie with many slices; two pies compared |
| the distribution of one variable | histogram, box plot, strip/dot plot | a bar of the mean alone |
| relationship between two variables | scatter (add a fitted line only if you state the fit) | dual-axis line of the two series |
| one number in context | a single number with its baseline and unit | a lone gauge with no scale |

Rules that hold across all of them:

- **Bar and column length encodes value from zero.** If zero is far away and the
  differences matter, use a dot plot (position, not length) instead of breaking
  the axis.
- **Position and length are read most accurately; area and color least.** Encode
  the value you most want read correctly in position or length; reserve color
  for categories, not for ordered magnitude (use a single-hue light-to-dark
  ramp if you must encode magnitude in color).
- **Sort bars by value** unless the category has its own order (time, size
  buckets). An alphabetical bar chart hides the ranking that is usually the
  point.
- **One series per line; label them directly** at the line's end rather than in
  a legend the reader has to cross-reference.

## Building it as self-contained SVG

The goal is one file that renders anywhere with nothing to fetch.

- Set a `viewBox` (for example `viewBox="0 0 640 400"`) and no fixed pixel
  width/height, so it scales to its container.
- Inline the data as the coordinates themselves; do not load it at render time.
- Compute every coordinate from the data and the axis scale, so the geometry is
  derived, not hand-placed. A hand-nudged bar is a bar you cannot read back.
- Keep text as `<text>`, not paths, so it stays selectable and accessible.
- Accessibility: give the root `<svg>` a `role="img"` and an `aria-label`, or a
  `<title>` and `<desc>` child, that state what the chart shows and the headline
  figure. Chart marks that carry values should have those values in text too,
  so a screen reader is not left with unlabeled rectangles.
- Draw the axes, the zero line, the tick labels with units, and a one-line note
  naming the data's source and date.
- Theme: either read `prefers-color-scheme` and set colors from CSS variables,
  or state the single background the colors assume. Do not hardcode black text
  that vanishes on a dark background.

A worked skeleton (bar), coordinates derived from the data and the scale:

```
values = [{label:"A", v:30}, {label:"B", v:80}, {label:"C", v:45}]
max = 80 ; plotH = 320 ; zeroY = 360        // baseline at the bottom
for each i, value:
  x   = 60 + i*80
  h   = (v / max) * plotH                    // length is proportional to value
  y   = zeroY - h                            // bars grow UP from the zero line
  <rect x=x y=y width=48 height=h/>          // height carries the value
  <text x=x+24 y=380>label</text>
axis: a line at zeroY, ticks at 0,20,40,60,80 with the unit
```

The read-back check (references/read-back.md) recomputes `v` from `height` and
the scale and confirms it matches the source. If your bars were placed by hand,
that check is what catches it.
