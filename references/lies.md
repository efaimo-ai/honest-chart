# The lies: how charts mislead while every number stays true

Each of these is a chart where the data is correct and the picture is not. They
are collected so you can recognize the shape before you draw it, and so "refuse
the lies" has concrete enemies.

## The truncated axis

A bar chart whose y-axis starts at 70 instead of 0 makes a 72-vs-78 difference
look like 4x. The bars' lengths no longer encode the values, only the values
minus 70. Fix: start bar and column axes at zero. If the interesting variation
genuinely lives in a narrow band far from zero, switch to a dot plot, where
POSITION (not length) carries the value and a non-zero axis is honest.

## The second y-axis

Put revenue on the left axis and complaints on the right, choose the two scales,
and you can make the two lines cross, diverge, or track each other at will. A
dual axis encodes a correlation you chose, not one the data has. Fix: two
separate small charts sharing an x-axis, or index both series to 100 at the
start and plot them on one scale.

## Area or bubble that grows as the square

A bubble whose RADIUS is proportional to the value makes a 2x value look 4x,
because area grows as the square of the radius. Same trap for a circle, a
pictogram scaled in both dimensions, or a "3D" bar with depth. Fix: scale AREA
to the value (radius proportional to the square root), or use length instead of
area.

## The cherry-picked window

A time series cropped to exactly the months that trend the way you want is every
number true and the whole picture false. Fix: show the full available range, or
state the window and why, and let the reader see what was left out.

## 3D and perspective

A 3D pie or bar tilts the marks so the ones in front read larger than their
value and the ones behind smaller. Perspective is decoration that distorts the
one thing a chart is for. Fix: never add a third dimension to two-dimensional
data.

## The rainbow scale on ordered data

A red-to-green or full-spectrum color ramp on an ordered quantity (temperature,
rate, rank) has no perceptual order: the reader cannot tell which color is more.
It also fails for the ~8% of men with color-vision deficiency. Fix: a single
hue, light to dark, for magnitude; distinct hues only for unordered categories.

## The pie with too many slices, or slices that do not sum

People compare angles poorly past a few slices, and a pie whose slices do not
add to the whole (overlapping categories, "other" dropped) is not a pie at all.
Fix: <= 5 slices with the rest grouped into a labeled "other", or a sorted bar
chart, which compares parts far more accurately.

## Overplotting

A scatter of 50,000 points is a solid blob that hides the density it claims to
show. Fix: transparency, hexbin or 2D-density, or sampling with the sample size
stated.

## Double encoding and chartjunk

Encoding one value in both height AND color AND label triples the ink for one
fact and invites a mismatch between them. Gridlines, heavy borders, backgrounds,
and drop shadows add ink that carries no data. Fix: one encoding per value;
remove any mark that would not change if the data changed.

---

The thread: in each, the numbers are right and the geometry is wrong, so the
reader takes away a ratio the data does not contain. The read-back check
(references/read-back.md) catches the geometric lies directly, because it
measures the ratio the picture actually shows.
