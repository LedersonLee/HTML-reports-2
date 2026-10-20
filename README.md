# Research HTML reports — 2

Public report hosting, split from the original `LedersonLee/html-reports` site
to avoid adding to its near-limit published footprint.

## HSSD semantic map compatibility review

- Report: `echoloc/hssd-semantic-compatibility/`
- All 152 scenes: train116 / val19 / test17, searchable split selector and thumbnails.
- Aligned geometry / original semantics / converted semantics, preserved internal/exterior scope, full-resolution zoom and integer-ID PNG download.
- Geometry, poses, splits, source annotations and existing ray caches are unchanged.
- Map IDs: empty0 / wall1 / window2 / door3; model ray IDs are different.
- Nonzero semantic on geometry-free pixels is a diagnostic, not automatic failure. Do not derive occupancy from semantic nonzero pixels.
- 760 published PNGs are checked against source hashes and raster dimensions. Only compact review images and sanitized provenance are published; no NPY, RGB, RIR or checkpoints.
- `publication.json` and `verification.json` record source conversion manifest identity and publication checks.

## HSSD corrected test comparison

## Seven acoustic input subsets

- Report: `echoloc/input-ablation-seven/`
- E / C / N / E+C / E+N / C+N / E+C+N; train12 / val3 / test3.
- Same 1,472 dense-test queries and four localization policies, independently
  verified against the original predictions; no retraining or inference here.
- Recall@0.1/0.5/1m, Recall@1m+30deg, yaw within10/30/50deg, median XY/yaw
  errors, and repair/harm counts; all metrics also available as CSV.
- Each condition has its own RGB/rays/likelihood viewer. Derived arrays use
  lossless gzip; identical published RGB/floorplans are reused with hash checks.
- Single seed and unequal parameter counts; exploratory test subset reuse.

## HSSD corrected test comparison details

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
