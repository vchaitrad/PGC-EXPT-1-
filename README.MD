# Experiment 1 — Parallel Matrix Multiplication

Implementation and benchmarking of a **4000 × 4000 matrix multiplication** problem using **Sequential** and **OpenMP** computing models.

## Overview

This experiment implements the *same* matrix multiplication problem on two computing models, so their performance can be compared directly on identical input:

| Part | Model | Parallelism | Where it runs |
|---|---|---|---|
| A | Sequential | None (baseline) | WSL2 Ubuntu, 1 CPU core |
| B | OpenMP | Shared-memory threads | WSL2 Ubuntu, 8 threads (32 logical CPUs available) |

Matrices **A** and **B** are initialized so every element is `1.0`; the expected result for every element of **C** is `4000.00`, which is used as the correctness check.

> Parts C (MPI) and D (CUDA) are in progress and will be added once those results are recorded.

## Results at a Glance

| Implementation | Resources | Execution Time | Verification |
|---|---|---|---|
| Sequential | 1 CPU core | 29.275032 s | C[0][0] = 4000.00, C[3999][3999] = 4000.00 |
| OpenMP | 8 threads (of 32 available) | 40.545825 s | C[0][0] = 4000.00 |

**Note on this run:** OpenMP came out *slower* than sequential here (29.28 s vs 40.55 s). This can happen due to thread-creation overhead, system load at the time of the run, or how the machine's 32 logical CPUs were scheduling the 8 requested threads. It's worth re-running both a couple of times and recording the most consistent values before final submission — a properly working OpenMP version is expected to be faster, not slower, than sequential.

## Repository Structure
