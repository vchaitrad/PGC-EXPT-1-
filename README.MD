# Experiment 1 — Parallel Matrix Multiplication

Implementation and benchmarking of a **4000 × 4000 matrix multiplication** problem across four computing models: **Sequential**, **OpenMP**, **MPI**, and **CUDA**.

## Overview

This experiment implements the *same* matrix multiplication problem four times, once per computing model, so the performance characteristics of each model can be compared directly on identical input:

| Part | Model | Parallelism | Where it runs |
|---|---|---|---|
| A | Sequential | None (baseline) | WSL2 Ubuntu, 1 CPU core |
| B | OpenMP | Shared-memory threads | WSL2 Ubuntu, 8 CPU threads |
| C | MPI | Distributed-memory processes | 4 Ubuntu VMs (1 Master + 3 Workers) |
| D | CUDA | GPU threads | NVIDIA GPU |

Matrices **A** and **B** are initialized so every element is `1.0`; the expected result for every element of **C** is `4000.00`, which is used as the correctness check across all four implementations.

## Results at a Glance

| Implementation | Resources | Execution Time | Speedup |
|---|---|---|---|
| Sequential | 1 CPU core | 244.120000 s | 1.00× |
| OpenMP | 8 CPU threads | 30.830434 s | 7.92× |
| MPI | 4 processes / 4 VMs | 92.979510 s | 2.63× |
| CUDA | NVIDIA RTX 4500 Ada | 0.165004 s | **1479.48×** |

Full analysis and observations: [`docs/06-results-and-comparison.md`](docs/06-results-and-comparison.md)

## Repository Structure
