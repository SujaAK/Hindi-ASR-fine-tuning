# Conversational Hindi ASR with Synthetic Speech

Research project: **Conversational Hindi ASR with Synthetic Speech: Data Generation, Quality Screening, and Evaluation on a Frozen Test Set.**

This repository tracks the study protocol, privacy-safe dataset manifests, experiment configurations, and verified evaluation results. Audio datasets, prediction-level transcripts, and model checkpoints are kept outside GitHub.

## Research status

- [x] Frozen Dataset v1 reconstructed and verified.
- [x] Source-grouped train/validation/test split frozen.
- [x] Qwen3-ASR-0.6B pretrained baseline inference completed.
- [x] Prediction coverage and reference alignment verified.
- [x] Overall raw WER/CER and raw subgroup metrics calculated and saved.
- [ ] Recover and apply the study's specified text-normalization implementation.
- [ ] Complete Whisper Medium pretrained baseline.
- [ ] Conduct the planned fine-tuning and comparative experiments.

## Frozen Dataset v1

Source dataset: [abdullahyousuf54/hindi-asr-all-augmented-v3](https://huggingface.co/datasets/abdullahyousuf54/hindi-asr-all-augmented-v3)

Pinned source revision:

`3c129569bb53f9847119753eb9fd3d91b640ee27`

A new source-grouped split was constructed for this study. It is not the original split supplied by the Hugging Face dataset.

| Property | Value |
|---|---:|
| Total rows | 10,746 |
| Unique source groups | 7,499 |
| Grouping key | Normalized `original_audio` |
| Stratification | `domain_tag × language_mix` |
| Random seed | 42 |
| Target proportions | 80% / 10% / 10% |

| Split | Rows | Source groups |
|---|---:|---:|
| Train | 8,599 | 6,001 |
| Validation | 1,072 | 752 |
| Test | 1,075 | 746 |
| **Total** | **10,746** | **7,499** |

Recorded verification:

- No source group crosses train, validation, and test.
- All stratification strata are represented in all three splits.
- Reconstructed row counts match the frozen split manifest.
- Reconstruction checks found zero missing IDs, absent IDs, domain mismatches, or language-metadata mismatches.

Frozen row-manifest SHA-256:

`e0d60e3fbbe3533cf6a1085e85fe02772295ec3f12ec730e17ddb48f9f851f42`

The repository contains privacy-safe split metadata and manifests, not audio payloads.

## Qwen3-ASR-0.6B pretrained baseline

### Experimental setup

| Item | Setting |
|---|---|
| Model | `Qwen/Qwen3-ASR-0.6B-hf` |
| Experiment | Pretrained baseline inference; no fine-tuning |
| Evaluation split | Frozen Dataset v1 test |
| Test samples | 1,075 |
| Audio sampling rate supplied to model | 16 kHz |
| Model loading precision | FP16 |
| Maximum generated tokens | 256 |
| Metric implementation | JiWER |
| Text normalization | None; raw transcript scoring |

The frozen test set was used for evaluation only. It was not used for training or checkpoint selection.

### Overall raw results

These scores were calculated without applying the study's planned text-normalization procedure.

| Metric | Result |
|---|---:|
| Test samples | 1,075 |
| Word error rate (WER) | **35.63%** |
| Character error rate (CER) | **15.35%** |

### Results by language mix

| Language mix | Samples | WER | CER |
|---|---:|---:|---:|
| Hindi | 912 | 36.81% | 15.49% |
| Hinglish | 163 | 29.22% | 14.59% |

### Results by difficulty bucket

| Difficulty | Samples | WER | CER |
|---|---:|---:|---:|
| Easy | 874 | 33.27% | 13.42% |
| Medium | 201 | 44.81% | 22.46% |

### Results by domain

| Domain | Samples | WER | CER |
|---|---:|---:|---:|
| Accessories | 18 | 46.43% | 24.45% |
| Dairy order | 98 | 36.59% | 16.25% |
| Delivery | 107 | 27.96% | 10.93% |
| General | 146 | 30.19% | 11.09% |
| Medical / pharmacy | 464 | 41.19% | 17.96% |
| Order | 93 | 28.42% | 10.34% |
| Payment | 94 | 29.17% | 14.73% |
| Recharge | 18 | 40.67% | 18.78% |
| Repair | 18 | 30.77% | 13.77% |
| Warranty | 19 | 27.32% | 13.74% |

Subgroup sizes are unequal. Results from small domains should be interpreted cautiously and are descriptive rather than strong standalone estimates.

### Prediction and reference integrity

The completed inference run passed these checks:

| Check | Result |
|---|---:|
| Expected test IDs | 1,075 |
| Saved prediction rows | 1,075 |
| Unique prediction IDs | 1,075 |
| Duplicate IDs | 0 |
| Missing predictions | 0 |
| Unexpected IDs | 0 |
| Required prediction columns | Present |
| Reference transcript mismatches | 0 |
| Errors during resumed inference | 0 |

### Evaluation caveat

These are **raw, unnormalized baseline scores**, not the final normalized scores intended for the paper. The research protocol specifies a normalization procedure for synthetic-test evaluation, including lowercase conversion, punctuation handling, and a digit-to-Hindi mapping.

The exact implementation must be recovered and applied consistently before normalized scores are reported or compared with normalized Whisper or fine-tuned-model scores. Raw and normalized results must remain separately labelled.

## Experiment artifacts and storage

The Qwen baseline prediction CSV, raw metrics JSON, and three subgroup metric CSVs were saved in Google Drive under:

`Hindi_ASR_Research/experiments/Qwen3_ASR_0.6B_baseline/`

Prediction-level transcripts and the recovery copy are intentionally not included in this repository.

GitHub is used for source code, experiment documentation, configurations, and privacy-safe manifests. Audio datasets and model checkpoints remain in controlled storage.

## Reproducibility principles

1. Use the pinned source dataset revision and frozen manifests.
2. Do not recreate or randomly resplit the frozen test set for later comparisons.
3. Use validation data for checkpoint selection; reserve the frozen test set for final evaluation.
4. Record model revision, library versions, decoding settings, hardware, and normalization version for each experiment.
5. Report raw and normalized metrics separately, with the exact normalization implementation identified.

## Repository layout

- `configs/` — frozen dataset and split protocol configurations.
- `manifests/` — privacy-safe frozen row manifest and source-group assignments.
- `README.md` — study protocol summary and verified Qwen pretrained-baseline results.
