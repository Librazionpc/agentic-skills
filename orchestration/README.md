# Orchestration Baseline (5–7 Specialist Team)

This folder defines the minimum orchestration layer that must be in place before scale-out.

## Required files
- `orchestrator.contract.yaml`: request/decision/specialist output contract
- `routing-matrix.yaml`: primary + secondary agent dispatch matrix

## Non-negotiable rules
1. Only orchestrator talks to the user.
2. Specialists return structured outputs only.
3. High-risk/compliance/fraud triggers auto-escalate.
4. No fabricated outcomes when tools fail.

## Suggested runtime flow
1. Ingest request envelope
2. Classify intent + risk
3. Dispatch primary (+ secondary when needed)
4. Collect specialist outputs
5. Merge with policy + risk gates
6. Return orchestrator final response or escalate
