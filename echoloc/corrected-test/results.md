# HSSD acoustic pilot: same-bank results

Exploratory 3-scene comparison; vision116-scene training vs acoustic12. No test fitting.

| Method | R@0.1m | R@0.5m | R@1m | R@1m/30deg | Median XY(m) | repair | harm |
|---|---:|---:|---:|---:|---:|---:|---:|
| vision_only | 14.74% | 43.14% | 45.92% | 44.77% | 2.28 | 0 | 0 |
| acoustic_only | 4.14% | 17.32% | 24.46% | 16.78% | 4.51 | 135 | 451 |
| equal_weight_fusion | 14.61% | 47.55% | 51.63% | 48.71% | 0.70 | 282 | 198 |
| visual_topk_acoustic_rerank | 12.02% | 45.86% | 51.43% | 49.25% | 0.81 | 267 | 186 |

## Observed results and failure analysis

- Vision R@1m=45.92%; Top-K=51.43%; delta=+5.50pp; paired-position CI=[+2.03,+9.11]pp.
- 이 pilot에서 Top-K 개선의 관측이 있습니다. 3개 scene 내 탐색적 결과이며 일반화된 필요성의 증명은 아닙니다.
- Top-K oracle R@1m=88.93%. 후보 누락=163; 후보가 있지만 음향 선택 실패=552.
- 전역 1:1 fusion R@1m=51.63%. 이 값이 Top-K와 같거나 높다면 후보 제한이 필수라는 근거는 더 약합니다.
- 기존 geometry QA 플래그는 검토 단서입니다. 자연스러운 clutter·ambiguity와 실제 데이터 불일치는 RGB/도면을 함께 확인해야 구분할 수 있습니다.
- Acoustic best epoch=5, last epoch=13. Train loss 감소만으로 unseen-scene 위치 추정이 학습됐다고 결론내리지 않습니다.

## Failure interpretation

- Top-K exclusion: the visual proposal pool has no candidate within1m; acoustics cannot repair this pool.
- Acoustic selection failure: Top-K includes a candidate within1m but the reranker chooses another location.
- Yaw-only failure: location within1m, heading outside30deg. The primary top-K rule retains visual heading.
- Geometry QA flags are existing review signals, not a confirmed cause. Inspect RGB/map/depth together.
- Pose-clustered paired CIs cover only these exploratory scenes, not unseen-scene generalization.

Detailed per-scene/K metrics, exclusions, input hashes and intervals: summary.json.
Per-query outcomes, diagnostic flags and scores: records.json / heatmaps/.
