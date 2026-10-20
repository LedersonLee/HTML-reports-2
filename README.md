# Research HTML reports — 2

Public report hosting, split from the original `LedersonLee/html-reports` site
to avoid adding to its near-limit published footprint.

## HSSD corrected test comparison

- Report: `echoloc/corrected-test/`
- Same three test scenes /1,472 queries as the original LAN report.
- Frozen vision-only, learned acoustic-only, fixed1:1 fusion and fixed Top-K.
- This is the earlier learned acoustic checkpoint, not the later three-input ablation model.
- Summary/CSV/Markdown are byte-identical copies. All query JSON, candidate banks
  and four-policy heatmaps are gzip-compressed losslessly and checked by SHA-256.
- RGB WebP display images and unchanged floorplans are reused from the original
  published HSSD subset report. No duplicated RGBs, raw RIRs or model checkpoints.
- `publication.json` records source/compressed hashes and all1,472 query checks.
- A modern browser with `DecompressionStream('gzip')` is required.

Existing report source data, original repository working tree, training queues
and model checkpoints were not modified for this publication.
