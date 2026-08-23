# Pinned fonts

The printed sheet is fitted to within a millimetre or two, so its font cannot
be left to whatever the printing machine happens to have installed. These files
fix the metrics, and therefore the line breaking and the panel fit, on any
machine — including one with no network.

| File | Source | Covers |
| --- | --- | --- |
| `liberation-sans-regular.woff2` | Liberation Sans Regular | Latin-1, Latin Extended-A, punctuation |
| `liberation-sans-bold.woff2` | Liberation Sans Bold | as above |
| `liberation-sans-italic.woff2` | Liberation Sans Italic | as above |

Liberation Sans is licensed under the SIL Open Font License 1.1, included here
as `OFL-1.1.txt`. The license permits redistribution of these subsets.

## Why Liberation Sans

The stylesheet used to lead with `Inter`, which was never actually loaded — no
`@font-face`, no webfont link — so on a machine without Inter installed every
proof of this layout silently rendered in Liberation Sans, the metric-compatible
Arial substitute. The panel fitting and the optical centring of the category
chips were both measured against that. Pinning Inter would therefore have
*changed* the layout rather than frozen it. Liberation Sans is what the design
was actually tuned to.

`Arial, Helvetica` trail the stack because they are metric-compatible with
Liberation Sans, so even a failed font load reflows nothing.

Note that U+26A0 is **not** in Liberation Sans. The warning triangles are inline
SVG (`_includes/icon-warning.html`) rather than a character, so they need no
font at all — but any warning glyph typed into the YAML would fall back.

## Rebuilding the subsets

Requires `fonttools` and `brotli`:

```sh
LATIN="U+0020-007E,U+00A0-00FF,U+0100-017F,U+2000-206F,U+2070-209F,U+20A0-20BF,U+2122,U+2190-2193,U+2212,U+2215"

for face in Regular Bold Italic; do
  pyftsubset "/usr/share/fonts/liberation/LiberationSans-$face.ttf" \
    --unicodes="$LATIN" --layout-features='*' --flavor=woff2 \
    --output-file="liberation-sans-$(echo $face | tr A-Z a-z).woff2"
done

```

If you add a medication whose name uses a character outside Latin Extended-A,
widen `LATIN` and rebuild — otherwise that one glyph falls back to a system font
and the pinning is defeated for it.
