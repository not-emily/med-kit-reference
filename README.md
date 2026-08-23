# Travel OTC Medication Reference

This Jekyll site is the master source for the printable travel medication reference card.

## Edit medication information

Edit `_data/medications.yml`.

Each category keeps a stable numeric ID (`1`–`6`). Individual medication bags use lettered IDs such as `1a`, `1b`, and `1c`.

## DayQuil / NyQuil

The supplied source card did not contain dosing instructions for these products. The project intentionally leaves those fields marked as pending until current product-label information is supplied.

## Local preview

```bash
bundle exec jekyll serve
```

## Printing

Use the browser's Print command. The print stylesheet is designed for 100% scale and landscape letter output. The physical fold/panel geometry can be refined after we choose the final fold pattern and exact sleeve dimensions.
