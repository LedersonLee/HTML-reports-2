# HSSD acoustic pilot: same-bank results

Exploratory 3-scene comparison; vision116-scene training vs acoustic12. No test fitting.

| Method | R@0.1m | R@0.5m | R@1m | R@1m/30deg | Median XY(m) | repair | harm |
|---|---:|---:|---:|---:|---:|---:|---:|
| vision_only | 14.74% | 43.14% | 45.92% | 44.77% | 2.28 | 0 | 0 |
| acoustic_only | 1.77% | 16.37% | 25.34% | 17.93% | 4.46 | 152 | 455 |
| equal_weight_fusion | 11.55% | 48.03% | 52.31% | 50.34% | 0.61 | 287 | 193 |
| visual_topk_acoustic_rerank | 8.63% | 44.16% | 50.68% | 47.89% | 0.88 | 251 | 181 |

## Observed results and failure analysis

- C + N · 동일한 frozen dense test bank. Best는 test를 열기 전에 val loss로 선택했습니다.

## Failure interpretation

- Top-K exclusion: the visual proposal pool has no candidate within1m; acoustics cannot repair this pool.
- Acoustic selection failure: Top-K includes a candidate within1m but the reranker chooses another location.
- Yaw-only failure: location within1m, heading outside30deg. The primary top-K rule retains visual heading.
- Geometry QA flags are existing review signals, not a confirmed cause. Inspect RGB/map/depth together.
- Pose-clustered paired CIs cover only these exploratory scenes, not unseen-scene generalization.

Detailed per-scene/K metrics, exclusions, input hashes and intervals: summary.json.
Per-query outcomes, diagnostic flags and scores: records.json / heatmaps/.
