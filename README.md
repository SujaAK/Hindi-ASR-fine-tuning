# Conversational Hindi ASR with Synthetic Speech

Research project: **Conversational Hindi ASR with Synthetic Speech: Data Generation, Quality Screening, and Evaluation on a Frozen Test Set.**

This repository tracks the study protocol, privacy-safe dataset manifests, experiment configurations, and verified evaluation results. Audio datasets, prediction-level transcripts, and model checkpoints are kept outside GitHub.

## Research status

- [x] Frozen Dataset v1 reconstructed and verified.
- [x] Source-grouped train/validation/test split frozen.
- [x] Qwen3-ASR-0.6B pretrained baseline inference completed.
- [x] Prediction coverage and reference alignment verified for both baselines.
- [x] Overall raw and normalized WER/CER calculated for both baselines.
- [x] Raw and normalized subgroup metrics and matched model comparisons calculated.
- [x] Whisper Medium pretrained baseline inference completed.
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

### Evaluation note

The raw Qwen scores above are retained as a separate scoring view. Normalized Qwen scores are now also available and are reported in the shared scoring and matched-comparison sections below. Raw and normalized results must remain separately labelled.

## Whisper Medium pretrained baseline

### Experimental setup

| Item | Setting |
|---|---|
| Model | `openai/whisper-medium` |
| Experiment | Pretrained baseline inference; no fine-tuning |
| Evaluation split | Frozen Dataset v1 test |
| Test samples | 1,075 |
| Language | Hindi (`hi`) |
| Task | Transcription |
| Precision | FP16 |
| Device | CUDA (`cuda:0`), NVIDIA T4 |
| Inference time | Approximately 25.42 minutes |
| Metric implementation | JiWER |

The inference metadata records 1,075 expected and saved predictions, zero inference errors, and the Hindi transcription configuration. A specific model revision was not recorded in the metadata.

### Prediction and reference integrity

| Check | Result |
|---|---:|
| Expected test IDs | 1,075 |
| Saved prediction rows | 1,075 |
| Unique prediction IDs | 1,075 |
| Duplicate IDs | 0 |
| Missing predictions | 0 |
| Unexpected IDs | 0 |
| Reference transcript mismatches | 0 |
| Inference errors | 0 |
| Coverage and reference alignment | PASS |

### Overall results

| Metric | Raw | Normalized |
|---|---:|---:|
| Test samples | 1,075 | 1,075 |
| WER | **50.77%** | **40.34%** |
| CER | **22.73%** | **19.61%** |

Exact raw values:

- WER: `0.5077366504854369`
- CER: `0.22726615119998714`

Normalized values:

- WER: approximately `0.403368`
- CER: approximately `0.196054`

### Language-group results — raw

| Language mix | Samples | WER | CER |
|---|---:|---:|---:|
| Hindi | 912 | 52.89% | 23.80% |
| Hinglish | 163 | 39.22% | 16.81% |

### Difficulty results — raw

| Difficulty | Samples | WER | CER |
|---|---:|---:|---:|
| Easy | 874 | 45.95% | 18.62% |
| Medium | 201 | 69.53% | 37.80% |

### Domain results — raw

| Domain | Samples | WER | CER |
|---|---:|---:|---:|
| Accessories | 18 | 54.29% | 25.55% |
| Dairy order | 98 | 50.24% | 18.49% |
| Delivery | 107 | 40.08% | 16.33% |
| General | 146 | 44.52% | 18.80% |
| Medical / pharmacy | 464 | 59.11% | 27.97% |
| Order | 93 | 42.48% | 17.67% |
| Payment | 94 | 37.03% | 14.98% |
| Recharge | 18 | 67.33% | 33.83% |
| Repair | 18 | 43.79% | 16.95% |
| Warranty | 19 | 38.25% | 13.74% |

### Language-group results — normalized

| Language mix | Samples | WER | CER |
|---|---:|---:|---:|
| Hindi | 912 | 41.69% | 20.49% |
| Hinglish | 163 | 32.94% | 14.82% |

### Difficulty results — normalized

| Difficulty | Samples | WER | CER |
|---|---:|---:|---:|
| Easy | 874 | 35.71% | 15.46% |
| Medium | 201 | 58.32% | 34.88% |

### Domain results — normalized

| Domain | Samples | WER | CER |
|---|---:|---:|---:|
| Accessories | 18 | 50.71% | 24.01% |
| Dairy order | 98 | 40.82% | 16.01% |
| Delivery | 107 | 33.13% | 14.09% |
| General | 146 | 33.73% | 16.12% |
| Medical / pharmacy | 464 | 46.61% | 24.47% |
| Order | 93 | 33.73% | 14.20% |
| Payment | 94 | 32.58% | 13.49% |
| Recharge | 18 | 40.67% | 19.28% |
| Repair | 18 | 33.73% | 14.64% |
| Warranty | 19 | 27.87% | 10.49% |

## Shared scoring and text normalization

WER and CER were computed using JiWER. The raw and normalized results are separate scoring views of the same frozen test examples.

### Raw scoring

Raw scoring compares the original reference and hypothesis strings without applying the project normalization function.

### Normalized scoring

The implemented normalization function applies the following operations to both references and hypotheses:

1. Lowercase conversion.
2. Whitespace collapsing and trimming.
3. Replacement of standalone Arabic-digit tokens using a finite 21-entry digit-to-Hindi mapping: 0–12, 15, 20, 25, 30, 50, 100, 500, and 1000.
4. Removal of the punctuation characters `। , . ! ? - : ; " '`.
5. Removal of timestamp-like tokens matching the implemented timestamp regular expression.
6. A final whitespace collapsing and trimming pass.

The digit mapping is a finite lookup table, not a general number-to-words converter. Only the listed values are mapped.

Raw and normalized scores must not be mixed. Comparisons should always use the same scoring protocol for both models.

## Matched baseline comparison: Qwen3-ASR-0.6B vs Whisper Medium

Both pretrained models were evaluated on the same 1,075 Frozen Dataset v1 test examples, with the same reference transcripts. Both prediction sets passed ID coverage and reference-alignment checks.

### Overall comparison

| Model | Samples | Raw WER | Raw CER | Normalized WER | Normalized CER |
|---|---:|---:|---:|---:|---:|
| Qwen3-ASR-0.6B | 1,075 | **35.63%** | **15.35%** | **17.42%** | **10.68%** |
| Whisper Medium | 1,075 | 50.77% | 22.73% | 40.34% | 19.61% |
| Whisper minus Qwen (percentage points) | — | +15.14 | +7.38 | +22.91 | +8.93 |

Lower WER and CER indicate fewer recognition errors. Qwen3-ASR-0.6B has lower overall WER and CER than Whisper Medium under both scoring protocols on this frozen test set.

### Language-group comparison — raw

| Language mix | Samples | Qwen WER | Qwen CER | Whisper WER | Whisper CER |
|---|---:|---:|---:|---:|---:|
| Hindi | 912 | 36.81% | 15.49% | 52.89% | 23.80% |
| Hinglish | 163 | 29.22% | 14.59% | 39.22% | 16.81% |

### Language-group comparison — normalized

| Language mix | Samples | Qwen WER | Qwen CER | Whisper WER | Whisper CER |
|---|---:|---:|---:|---:|---:|
| Hindi | 912 | 17.73% | 10.58% | 41.69% | 20.49% |
| Hinglish | 163 | 15.74% | 11.20% | 32.94% | 14.82% |

### Difficulty comparison — raw

| Difficulty | Samples | Qwen WER | Qwen CER | Whisper WER | Whisper CER |
|---|---:|---:|---:|---:|---:|
| Easy | 874 | 33.27% | 13.42% | 45.95% | 18.62% |
| Medium | 201 | 44.81% | 22.46% | 69.53% | 37.80% |

### Difficulty comparison — normalized

| Difficulty | Samples | Qwen WER | Qwen CER | Whisper WER | Whisper CER |
|---|---:|---:|---:|---:|---:|
| Easy | 874 | 14.99% | 8.75% | 35.71% | 15.46% |
| Medium | 201 | 26.86% | 17.74% | 58.32% | 34.88% |

### Domain comparison — raw

| Domain | Samples | Qwen WER | Qwen CER | Whisper WER | Whisper CER |
|---|---:|---:|---:|---:|---:|
| Accessories | 18 | 46.43% | 24.45% | 54.29% | 25.55% |
| Dairy order | 98 | 36.59% | 16.25% | 50.24% | 18.49% |
| Delivery | 107 | 27.96% | 10.93% | 40.08% | 16.33% |
| General | 146 | 30.19% | 11.09% | 44.52% | 18.80% |
| Medical / pharmacy | 464 | 41.19% | 17.96% | 59.11% | 27.97% |
| Order | 93 | 28.42% | 10.34% | 42.48% | 17.67% |
| Payment | 94 | 29.17% | 14.73% | 37.03% | 14.98% |
| Recharge | 18 | 40.67% | 18.78% | 67.33% | 33.83% |
| Repair | 18 | 30.77% | 13.77% | 43.79% | 16.95% |
| Warranty | 19 | 27.32% | 13.74% | 38.25% | 13.74% |

### Domain comparison — normalized

| Domain | Samples | Qwen WER | Qwen CER | Whisper WER | Whisper CER |
|---|---:|---:|---:|---:|---:|
| Accessories | 18 | 30.71% | 21.45% | 50.71% | 24.01% |
| Dairy order | 98 | 18.71% | 12.20% | 40.82% | 16.01% |
| Delivery | 107 | 11.78% | 6.09% | 33.13% | 14.09% |
| General | 146 | 13.33% | 6.62% | 33.73% | 16.12% |
| Medical / pharmacy | 464 | 21.12% | 12.93% | 46.61% | 24.47% |
| Order | 93 | 10.32% | 5.42% | 33.73% | 14.20% |
| Payment | 94 | 15.80% | 11.47% | 32.58% | 13.49% |
| Recharge | 18 | 20.00% | 13.32% | 40.67% | 19.28% |
| Repair | 18 | 13.61% | 10.08% | 33.73% | 14.64% |
| Warranty | 19 | 8.74% | 9.24% | 27.87% | 10.49% |

### Descriptive interpretation

- Qwen3-ASR-0.6B has lower overall WER and CER than Whisper Medium under both raw and normalized scoring on this test set.
- Qwen has lower WER in every reported language, difficulty, and domain subgroup. It also has lower CER in every subgroup except raw warranty, where both models display 13.74% CER.
- Medium-difficulty examples have higher error rates than easy examples for both systems.
- Medical/pharmacy is the largest domain subgroup (464 examples) and has relatively high error rates for both models.
- Several domains contain only 18 or 19 examples. Their subgroup scores are descriptive and should be interpreted cautiously.
- These are descriptive results on one frozen test set. No confidence intervals or statistical significance tests are reported here; the observed differences should not be described as statistically significant.

## Baseline experiment artifacts

The experiment outputs are retained in Google Drive under:

`Hindi_ASR_Research/experiments/`

### Qwen3-ASR-0.6B baseline

Directory: `Qwen3_ASR_0.6B_baseline/`

Includes the test prediction CSV, raw and normalized overall metric JSON files, and raw and normalized subgroup metric CSV files.

### Whisper Medium baseline

Directory: `Whisper_Medium_baseline/`

Includes the test prediction CSV, inference metadata, error log, raw and normalized overall metric JSON files, and raw and normalized subgroup metric CSV files.

### Matched comparisons

Directory: `baseline_comparison/`

Includes overall, language-mix, domain, and difficulty comparison CSVs for both raw and normalized scoring.

Prediction-level transcripts and audio files are intentionally not included in this repository. Model checkpoints and large experiment outputs remain in controlled storage.

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
- `README.md` — study protocol, frozen dataset details, and verified pretrained-baseline results.
