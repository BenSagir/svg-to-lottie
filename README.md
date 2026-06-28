# Figma SVG → Lottie converter

A single-file, offline web tool that converts a CSS-animated SVG (Figma / Jitter
export with a `<style>` block of `@keyframes`) into a Lottie JSON that
`lottie-react-native` and lottie-web can play. `react-native-svg` can't run CSS
animations, so this re-expresses the artwork + motion as Lottie.

## Use

Open `index.html` in any browser (double-click it — no server needed). Drop an
`.svg`, preview the result, download the `.json`. Nothing is uploaded; all
conversion happens locally.

### Options

- **FPS** — frame rate of the Lottie comp (default 60).
- **Precision** — decimal places for coordinates; lower = smaller file.
- **Margin px** — padding added around the artwork. Figma exports use
  `overflow: visible`, so the bob animation / edge decorations spill past the
  viewBox; a Lottie comp clips to its bounds, so the margin grows the comp so
  nothing clips. Bump it if the top or a corner element looks cut.

## Supported / limits

- Absolute path commands `M L H V C Z` (fails loudly on arcs/quadratics/shortcuts
  — flatten them in the export first).
- `translateX` / `translateY` bob and rotation-about-a-pivot
  (`translate(p) rotate(Nrad) translate(-p)`) are converted.
- CSS motion-path animations (`offset-path` + `offset-distance`) aren't followed;
  those elements are placed statically at their base position.

The conversion core is the same logic as `EchSham/scripts/svg-anim-to-lottie.mjs`
(verified byte-identical).
