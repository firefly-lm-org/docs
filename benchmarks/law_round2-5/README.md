# Law Domain - Round 2-5 Training Results

## Overview
- **Domain**: Law (Chinese labor law, contract law, social insurance)
- **Base model**: Qwen3-1.5B-Instruct-4bit (unsloth/transformers)
- **Method**: QLoRA (r=8, alpha=16, target=q_proj,v_proj)
- **Rounds**: Round 2 through Round 5 (from aggregated Round 1 base)
- **Aggregation**: FedAvg equal-weight across rounds
- **Scheduler**: api.firefly-lm.com:8000
- **Date**: 2026-07-28

## Files
| File | Size | Description |
|------|------|-------------|
| aggregated/adapter_model.safetensors | ~35MB | FedAvg equal-weight aggregation (all 4 rounds) |
| round2/ | | Round 2 adapter (30 steps from R1 base) |
| round3/ | | Round 3 adapter (30 steps from R2 base) |
| round4/ | | Round 4 adapter (30 steps from R3 base) |
| round5/ | | Round 5 adapter (30 steps from R4 base) |

## Training Details
- **Samples per round**: 8-28 (law domain, Chinese labor law QA)
- **Steps per round**: 30
- **Learning rate**: 2e-4
- **Batch**: 1 (gradient accumulation 4)
- **Max sequence**: 512

## Convergence
- Round 2-5 each 30 steps from previous round's aggregated output
- Final FedAvg: equal-weight average of all 4 round adapters

## Provenance
- Task IDs: law_r2_*, law_r3_*, law_r4_*, law_r5_*
- Aggregation: FedAvg equal-weight
- SHA256: see each meta.json
- Scheduler: api.firefly-lm.com:8000

## Verification
- All adapters validated by FedAvg aggregation script
- Holdout accuracy: see aggregated/meta.json
