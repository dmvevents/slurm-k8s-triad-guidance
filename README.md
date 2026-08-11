# Slurm-on-Kubernetes Triad — SUNK vs Slinky vs a homegrown port

A published desk analysis of three ways to run Slurm on Kubernetes, evaluated for an AWS P5/P5en + EFA fleet.

**Live site:** https://dmvevents.github.io/slurm-k8s-triad-guidance/

## Why this exists

Naming in this space is misleading. One option is an installer for an engine you cannot see; another is named
for a platform it does not actually integrate with. This site states what is in each artifact, tags every claim
with its source, and marks as `UNVERIFIED` the things we could not substantiate — rather than guessing.

## The short version

| | Open source | EFA on AWS | Dynamic scale | Maturity |
|---|---|---|---|---|
| **SUNK** (`coreweave/sunk-anywhere`) | wrapper only; engine proprietary | **none in the open recipe** (plain TCP NCCL) | partial (pod layer) | 1 commit, no releases |
| **Slinky** (`SlinkyProject/slurm-operator`) | **fully open, Apache-2.0** | **wired** via `awslabs/ai-on-eks` blueprint | **yes** (NodeSet↔0 + HPA + Karpenter) | 1,253+ commits, v1.2.0 |
| **Ours** (`slurm-on-eks-hyperpod`) | ours, custom | env-var only, no EFA device | no as shipped; CLOUD-autoscale PR staged (hermetic tests only) | 2-node demonstrator |

## Honest scope

Desk analysis plus one live arm. **T1 (NCCL busbw at scale) has been run** on a 2×p5en pair
(2026-07-30): raw-MPIJob ceiling 476.95 GB/s, static-manifest arm 477.95 (parity), operator-managed
arm 452.17 (94.8%) — see the matrix and landing verdict. The remaining arms — small-training
communication scalability and scale-up latency — are **designed but not yet run**. No number appears
on the site before it has actually been taken. See section 04 for the pre-registered gates.

## Structure

- `index.html` — overview, the three findings, where each option wins, dynamic-scale verdict
- `sections/01-what-each-is.html` — what is actually in each artifact
- `sections/02-matrix.html` — ten-dimension evaluation matrix, every cell sourced
- `sections/03-dynamic-scale.html` — the two scaling layers, and drain-awareness
- `sections/04-test-plan.html` — the live arms and their pre-registered gates
