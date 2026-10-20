# HSSD acoustic pilot: same-bank results

Exploratory 3-scene comparison; vision116-scene training vs acoustic12. No test fitting.

| Method | R@0.1m | R@0.5m | R@1m | R@1m/30deg | Median XY(m) | repair | harm |
|---|---:|---:|---:|---:|---:|---:|---:|
| vision_only | 14.74% | 43.14% | 45.92% | 44.77% | 2.28 | 0 | 0 |
| acoustic_only | 4.01% | 14.88% | 22.35% | 6.18% | 4.79 | 139 | 486 |
| equal_weight_fusion | 13.38% | 42.66% | 46.40% | 43.55% | 1.63 | 254 | 247 |
| visual_topk_acoustic_rerank | 12.02% | 44.16% | 50.68% | 48.85% | 0.90 | 259 | 189 |

## Observed results and failure analysis

- STFT energy · 동일한 frozen dense test bank. Best는 test를 열기 전에 val loss로 선택했습니다.

## Failure interpretation

- Top-K exclusion: the visual proposal pool has no candidate within1m; acoustics cannot repair this pool.
- Acoustic selection failure: Top-K includes a candidate within1m but the reranker chooses another location.
- Yaw-only failure: location within1m, heading outside30deg. The primary top-K rule retains visual heading.
- Geometry QA flags are existing review signals, not a confirmed cause. Inspect RGB/map/depth together.
- Pose-clustered paired CIs cover only these exploratory scenes, not unseen-scene generalization.

Detailed per-scene/K metrics, exclusions, input hashes and intervals: summary.json.
Per-query outcomes, diagnostic flags and scores: records.json / heatmaps/.
