# Changelog

All notable changes to honest-chart are documented here. Format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/); this project uses
[Semantic Versioning](https://semver.org/).

## [0.1.0] - 2026-11-16

> **Published without provenance.** This is the first version of `honest-chart` on
> npm, and it came from an authenticated session: npm's trusted publishing is
> configured from an existing package's settings page, so it cannot create a
> package that has never been published. From the next version the release
> workflow publishes it, with SLSA provenance naming the commit that built it.

### Added

- **The skill is installable.** `npx honest-chart` copies `SKILL.md` and its
  `references/` into `./.claude/skills/honest-chart/`, with `--global`, `--dir`,
  `--check`, `--print`, `--uninstall` and `--force`. The installer reads every
  byte back after writing it and fails if what landed is not what it wrote, and
  it prints how many files it installed rather than only that it succeeded.
- Three exit codes rather than two: 0 installed or already identical, 1 present
  and different, 2 could not run at all. "It differs" and "I could not do this"
  are different answers and collapsing them is how a broken install reads as a
  clean one.
- CI on four cells (Ubuntu and Windows, Node 20 and 24), including an end to end
  run that packs the package the way npm would, installs it from that tarball,
  and installs the skill from there.

### Changed

- Reading back covers charts a library drew, not only hand-written SVG:
  matplotlib (`ax.patches`, `get_ydata()`, `get_ylim()`), plotly, Vega-Lite and
  Recharts (the spec's values and its axis settings), and spreadsheet axis
  options. The read-back ends in a short table of source and read values.
- "Build it self-contained" became "renders the same anywhere": data in the
  chart, an exact library version when there is a library, instead of
  forbidding any library from a CDN.
- The description names the libraries and leaves out diagrams with no data and
  library syntax questions.

### Notes

- The package version and the version in `SKILL.md` frontmatter are one number,
  pinned by a test.

[0.1.0]: https://github.com/efaimo-ai/honest-chart/releases/tag/v0.1.0
