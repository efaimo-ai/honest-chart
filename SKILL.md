---
name: honest-chart
description: Use when making a chart or plot from numbers, or reviewing one before it is shared - bar, line, pie, scatter, area, bubble or stacked, in SVG, HTML, matplotlib, plotly, Recharts, Vega-Lite or a spreadsheet. Catches the ways a chart misleads while every number is correct - a bar axis that skips zero, two y-axes, bubbles scaled by radius, a cherry-picked window, a rainbow scale on ordered data. Ends by reading the values back off the rendered chart and comparing them with the source. Not for diagrams without data or for library syntax questions.
license: Apache-2.0
metadata:
  version: "0.1.0"
  homepage: "https://efaimo.com"
  verified_against: "2026-09-04"
---

# honest-chart

A chart is a claim: that the picture is proportional to the numbers. It is a
checkable claim, and it is almost never checked. Most charting advice is about
making the picture attractive. This is about making it TRUE, and proving it did.

Three moves: encode honestly, refuse the known lies, then read the chart back.

## 1. Encode honestly

Pick the chart for the data's shape, not the one that looks impressive. The map
(comparison, trend, part-of-whole, distribution, relationship) and how to build
each as clean, self-contained SVG is in
[references/encodings.md](references/encodings.md). The same rules hold for a
chart a library draws.

Then hold the encoding to the data:

- **Bar length starts at zero.** A bar's length is its value; start the axis
  anywhere else and the length lies about the ratio.
- **Area scales with the value, not its square.** Doubling a value doubles the
  area, never the radius. Bubble and area charts break this by default.
- **One scale per axis.** A second y-axis lets you make any two series look
  correlated by choosing the scales. Do not.
- **Label the axes, the units, and the source.** A number with no unit and no
  provenance is decoration, not evidence.

## 2. Refuse the lies

Before you draw, check the plan against [references/lies.md](references/lies.md):
the specific ways a chart deceives (truncated axis, dual axis, 3D and
perspective, a hand-picked time window, a rainbow scale on ordered data,
overplotting, a pie with too many slices). Each distorts in a named way, and
each has a fix that keeps the chart honest without hiding the point.

## 3. Read it back

Measure the encoded values back out of the finished chart and compare them
with the source data. Read from what was rendered:

- Hand-written or exported SVG: rect heights, arc sweep angles and point
  coordinates, per [references/read-back.md](references/read-back.md).
- matplotlib: after drawing, `[(p.get_x(), p.get_height()) for p in ax.patches]`
  for bars and `line.get_ydata()` for lines, plus `ax.get_ylim()` for the axis.
  Save with `format="svg"` when you need the geometry itself.
- plotly, Vega-Lite, Recharts: the spec is data. Compare its values with the
  source and check the axis in the same spec (`layout.yaxis.range`,
  `scale.zero`, the `YAxis` `domain`). The rendered SVG in the page can be
  measured like any other; the library details are in the same reference.

Report it as a short table (label, source value, value read back) with the
largest gap. A pixel of rounding is fine; anything larger is a bug in the
chart.

| chart | the measurement that proves it |
|---|---|
| bar | bar pixel length / axis scale == the value |
| pie / donut | slice sweep angle / 360 == value / total |
| line | each plotted point's y == the value at that x |
| stacked | segments sum to the total, and each segment == its own value |

## Build it so it renders the same anywhere

Put the data in the chart, inline or in a file shipped with it. If you use a
library, pin an exact version. Give it a `viewBox` or a fixed size,
selectable text and an accessible name (`<title>` and `<desc>`, or
`role="img"` with a label), and state the theme it assumes if it is not
theme-aware.

## The tells (a chart that is probably lying)

- a bar chart whose y-axis does not start at zero;
- two y-axes on one plot;
- a pie with more than five slices, or slices that do not sum to the whole;
- a bubble or area whose size grows as the square of the value;
- a time series cropped to exactly the window that makes the point;
- a value with no unit and no source.

## Scope

Color, type and layout belong to whatever design guidance you already follow.
This covers whether the geometry matches the numbers, and leaves whether the
data is worth charting to you.

<!-- generated:siblings -->

## Siblings

Every skill in this set is about a report that was true about the wrong thing. The set: https://efaimo.com/skills

- `red-before-green` - the general form of move 3: do not trust a clean result until you have watched it fail.
- `read-it-back` - reading a written artifact back to see what actually landed, here applied to a rendered figure.

<!-- /generated:siblings -->
