# HSSD acoustic pilot: same-bank results

Exploratory 3-scene comparison; vision116-scene training vs acoustic12. No test fitting.

| Method | R@0.1m | R@0.5m | R@1m | R@1m/30deg | Median XY(m) | repair | harm |
|---|---:|---:|---:|---:|---:|---:|---:|
| vision_only | 14.74% | 43.14% | 45.92% | 44.77% | 2.28 | 0 | 0 |
| acoustic_only | 2.99% | 17.19% | 24.25% | 8.02% | 4.39 | 136 | 455 |
| equal_weight_fusion | 10.46% | 42.32% | 45.99% | 44.02% | 1.81 | 234 | 233 |
| visual_topk_acoustic_rerank | 8.76% | 46.13% | 51.22% | 48.71% | 0.82 | 258 | 180 |

## Observed results and failure analysis

- Mic-mean-removed envelope · 동일한 frozen dense test bank. Best는 test를 열기 전에 val loss로 선택했습니다.

## Failure interpretation

- Top-K exclusion: the visual proposal pool has no candidate within1m; acoustics cannot repair this pool.
- Acoustic selection failure: Top-K includes a candidate within1m but the reranker chooses another location.
- Yaw-only failure: location within1m, heading outside30deg. The primary top-K rule retains visual heading.
- Geometry QA flags are existing review signals, not a confirmed cause. Inspect RGB/map/depth together.
- Pose-clustered paired CIs cover only these exploratory scenes, not unseen-scene generalization.

Detailed per-scene/K metrics, exclusions, input hashes and intervals: summary.json.
Per-query outcomes, diagnostic flags and scores: records.json / heatmaps/.
