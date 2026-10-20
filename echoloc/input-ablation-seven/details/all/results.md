# HSSD acoustic pilot: same-bank results

Exploratory 3-scene comparison; vision116-scene training vs acoustic12. No test fitting.

| Method | R@0.1m | R@0.5m | R@1m | R@1m/30deg | Median XY(m) | repair | harm |
|---|---:|---:|---:|---:|---:|---:|---:|
| vision_only | 14.74% | 43.14% | 45.92% | 44.77% | 2.28 | 0 | 0 |
| acoustic_only | 5.50% | 24.12% | 31.11% | 22.42% | 3.61 | 186 | 404 |
| equal_weight_fusion | 17.46% | 53.33% | 56.73% | 55.03% | 0.38 | 317 | 158 |
| visual_topk_acoustic_rerank | 15.01% | 53.33% | 57.88% | 55.91% | 0.41 | 310 | 134 |

## Observed results and failure analysis

- Energy + coherence + envelope · 동일한 frozen dense test bank. Best는 test를 열기 전에 val loss로 선택했습니다.

## Failure interpretation

- Top-K exclusion: the visual proposal pool has no candidate within1m; acoustics cannot repair this pool.
- Acoustic selection failure: Top-K includes a candidate within1m but the reranker chooses another location.
- Yaw-only failure: location within1m, heading outside30deg. The primary top-K rule retains visual heading.
- Geometry QA flags are existing review signals, not a confirmed cause. Inspect RGB/map/depth together.
- Pose-clustered paired CIs cover only these exploratory scenes, not unseen-scene generalization.

Detailed per-scene/K metrics, exclusions, input hashes and intervals: summary.json.
Per-query outcomes, diagnostic flags and scores: records.json / heatmaps/.
