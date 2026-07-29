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
| **Ours** (`slurm-on-eks-hyperpod`) | ours, custom | env-var only, no EFA device | no (fixed replicas, static conf) | 2-node demonstrator |

## Honest scope

This is **desk analysis**. The performance arms — NCCL busbw at scale, small-training communication
scalability, and scale-up latency — are **designed but not yet run** (they need a free 2×p5en pair). No
measured performance number appears on the site until it has actually been taken. See section 04 for the
pre-registered gates.

## Structure

- `index.html` — overview, the three findings, where each option wins, dynamic-scale verdict
- `sections/01-what-each-is.html` — what is actually in each artifact
- `sections/02-matrix.html` — ten-dimension evaluation matrix, every cell sourced
- `sections/03-dynamic-scale.html` — the two scaling layers, and drain-awareness
- `sections/04-test-plan.html` — the live arms and their pre-registered gates
