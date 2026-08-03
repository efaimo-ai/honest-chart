# Read it back: prove the render matches the data

The chart claims its geometry is proportional to the data. An SVG is text with
numbers in it, so that claim is measurable. Read the encoded values back out of
the finished chart and confirm they match the source within a small tolerance
(a pixel of rounding is fine; a 20% gap is a bug in the chart, not the data).

Do this against the RENDERED artifact, not the code that was supposed to draw
it. A hand-nudged coordinate, an off-by-one baseline, a scale applied twice, a
mislabeled axis: none of these show up in the source's intent, all of them show
up when you measure the picture.

## Bar and column

Each bar's length encodes its value from the zero line.

```
value_read = (bar_length_px / plot_height_px) * axis_max
```

- `bar_length_px` is the rect's `height` (column) or `width` (bar).
- `plot_height_px` is the pixel distance from the zero line to the axis max.
- Confirm `value_read` matches the source for every bar, and that the zero line
  really is at the value 0 on the axis (the most common lie is that it is not).

## Pie and donut

Each slice's swept angle encodes its share of the total.

```
share_read = slice_sweep_degrees / 360
```

Confirm `share_read` matches `value / sum(values)` for each slice, and that the
slices sum to 360 degrees (to the whole). If they do not sum, the chart is
claiming a part-of-whole that is not whole.

## Line and scatter

Each plotted point's y-pixel maps back through the axis scale to its value.

```
value_read = axis_min + ((baseline_y - point_y_px) / plot_height_px) * (axis_max - axis_min)
```

Confirm each point's `value_read` matches the source value at that x. For a
line, also confirm no point was invented between data points (a smoothed spline
that bulges past the data is drawing values that do not exist).

## Stacked bar

Two checks: each segment's length reads back to its own value (as for a plain
bar), AND the segments sum to the stack total. A stack whose segments do not sum
to the total shown is double-counting or dropping a category.

## Area and bubble

Measure the mark's AREA, not its radius or height, and confirm area is
proportional to the value.

```
area_ratio = area(mark_a) / area(mark_b)
value_ratio = value_a / value_b
```

`area_ratio` should equal `value_ratio`. If instead the RADIUS ratio equals the
value ratio, the areas are off by the square and the chart exaggerates.

## What a failed read-back means

The picture and the numbers disagree, and the picture is what the reader
believes. Fix the chart until the read-back passes, then say, in the chart's own
note, that the values were verified against the source. That sentence is only
worth writing because you did the measurement.
