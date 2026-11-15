# Metrics and validation

[← Project overview](../README.md)

## Results status

**Real baseline results are pending.** No human-reviewed trial measurements were available in the reviewed implementation snapshot. The concept video and its scripted perception decisions are excluded from performance evaluation.

| Metric | Definition | Measured value |
| --- | --- | --- |
| Correct recovery | At least one independently correct current MATCH + TRACK after first reappearance | Pending |
| Loss-to-confirmation | First correct confirmation minus true disappearance time | Pending |
| Visible-to-confirmation | First correct confirmation minus first visible-again time | Pending |
| Recovery @ 5/10/20/30 s | Correct confirmation within the deadline after disappearance | Pending |
| Wrong confirmation after loss | Any independently wrong MATCH + TRACK after loss | Pending |
| Visible sample coverage | Correctly confirmed visible sample frames / reviewed visible sample frames | Pending |
| False-present sample rate | Confirmed absent-target sample frames / reviewed absent-target sample frames | Pending |
| Prediction availability | Non-null estimates / processed frames in the first 1.5 s of annotated loss | Pending |
| Forecast center error | Frozen position/velocity forecast versus independent future box center, also compared to hold-last-position | Pending |
| Processed FPS | Processed frame intervals per capture/media second | Pending |
| p50/p95 overlay-log latency | Capture timestamp to overlay-log event | Pending |

Stationarity declarations and zero-velocity prediction fractions are diagnostic fields. They do not establish prediction accuracy.

## What is recorded

The native recording tool captures clean processed frames, the existing overlay video, timestamped frame metadata, system events, a run manifest, and configuration/model/source fingerprints. Independent human labels live in a separate file that the runtime does not read. Reports join decisions to reviewed ground truth and recompute trial/group CSVs.

It records frames consumed by the pipeline, not every webcam sensor frame. Overlay-log latency is not sensor-to-display latency, and replay media FPS is not processing throughput. Sensor drops and physical travel are not measured by this recorder.

## Baseline protocol

- Three trials each: continuous visibility, short occlusion, long occlusion, reversal, and distractor while the target is absent.
- Freeze camera, scene, target, lighting, thresholds, weights, and instrumentation for the baseline.
- Enroll one person once and use one designed loss interval per recovery trial.
- Review target visibility and identity correctness independently from system output.
- Keep failed and interrupted trials in the inventory; define exclusions explicitly.
- Continue no-return trials for at least 30 seconds after loss to evaluate all recovery deadlines.

A trial ending before a deadline is censored at that deadline, not automatically a failure or success. Report evaluable counts and Wilson 95% intervals. Success-only recovery-time medians must be paired with failures and sample sizes.

The forecast diagnostic uses continuous visible footage with future observations withheld from each frozen anchor. It tests the constant-velocity baseline against independently annotated future centers; it does not prove actual hidden trajectories or constitute a standard tracking benchmark.

## Engineering validation

| Evidence | Scope and limitation |
| --- | --- |
| Private validation report: 188 passed, one skipped | Source-reported full-suite result; not rerun in full for this showcase |
| Source-reported Chromium fixture checks | Synthetic capture/embeddings exercised UI enrollment, reference, states, prediction, and lifecycle; not real ReID accuracy |
| Source-reported three-frame hardware smoke | Exercised real camera/model startup and JPEG transport; did not enroll a person |
| Showcase map check | Actual controller/planner reached TRACK, then MOVE with a feasible obstacle-avoiding route in supplied example geometry |
| Concept animation | Scripted visual explanation; perception numbers are not measurements |

Real selected-person occlusion recovery, recorded-video recognition, and physical movement remain unvalidated in the reviewed notes. Docker configuration validation does not establish a successful image build or deployment.

## Publication policy

Publish a real trial only with its condition, code/config identity, independent review scope, success/failure status, and denominator. Keep generated media clearly identified and separate from measured results.

