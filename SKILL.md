---
name: honest-chart
description: Use when turning data into a chart, graph, plot, or any data visualization - a bar, line, pie, scatter, or an SVG figure of numbers - and before presenting it. Builds the chart so its geometry is proportional to the data, refuses the ways charts mislead (a truncated axis, a second y-axis, area that does not scale with value, a cherry-picked window), and verifies the finished chart by reading its values back off the render and checking they match the source.
license: Apache-2.0
metadata:
  version: "0.1.0"
  homepage: "https://efaimo.ai"
  verified_against: "2026-08-03"
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
[references/encodings.md](references/encodings.md).

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

An SVG is measurable text. After you render, measure the encoded values back OUT
of the chart - the bar heights, the pie arc angles, the line's plotted points -
and confirm they match the source data within a small tolerance. A chart you
have not read back is an unverified claim. This is the "look at the artifact,
not the spec" that the rest of these tools run on, applied to the one claim a
chart makes. The measurements are in
[references/read-back.md](references/read-back.md).

| chart | the measurement that proves it |
|---|---|
| bar | bar pixel length / axis scale == the value |
| pie / donut | slice sweep angle / 360 == value / total |
| line | each plotted point's y == the value at that x |
| stacked | segments sum to the total, and each segment == its own value |

## Build it self-contained

The chart should be one artifact with nothing to fetch: inline the data, use a
`viewBox` so it scales, keep text selectable, make it accessible (a `<title>`
and `<desc>`, or `role="img"` with a label), and either be theme-aware or state
the single theme it assumes. A chart that needs a CDN to render is a chart that
breaks the moment it is shared.

## The tells (a chart that is probably lying)

- a bar chart whose y-axis does not start at zero;
- two y-axes on one plot;
- a pie with more than five slices, or slices that do not sum to the whole;
- a bubble or area whose size grows as the square of the value;
- a time series cropped to exactly the window that makes the point;
- a value with no unit and no source.

## What this is not

Not a charting library; there is nothing to install. It is the discipline that
makes a chart you generate say what the data says, and lets you prove it did. It
does not decide whether the data is worth charting - that is your call - only
that the picture does not misstate it.

## Related

- [`red-before-green`](https://github.com/efaimo-ai/red-before-green) is the
  general form of the third move: do not trust a clean result until you have
  watched it fail. `honest-chart` applies it to the specific claim a chart makes.
- [efaimo](https://github.com/efaimo-ai/efaimo) audits the quality and context
  cost of MCP servers and Agent Skills, including this one.
