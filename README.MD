
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

## How to Run Each Part

```bash
# Part A — Sequential
cd sequential
gcc -O2 matrix_sequential.c -o matrix_sequential
./matrix_sequential

# Part B — OpenMP
cd openmp
export OMP_NUM_THREADS=8
gcc -O2 -fopenmp matrix_openmp.c -o matrix_openmp
./matrix_openmp

# Part C — MPI (run from the Master node, after copying the binary to each worker)
cd mpi
mpicc -O2 matrix_mpi.c -o matrix_mpi
scp matrix_mpi worker1:~/matrix_mpi
scp matrix_mpi worker2:~/matrix_mpi
scp matrix_mpi worker3:~/matrix_mpi
mpirun -np 4 --hostfile hosts sh -c '$HOME/matrix_mpi'

# Part D — CUDA
cd cuda
nvcc -O2 matrix_cuda.cu -o matrix_cuda
./matrix_cuda
```

Detailed, step-by-step instructions are in [`docs/`](docs/) — start at [`docs/01-introduction.md`](docs/01-introduction.md).

## Conclusion

The experiment implements a single matrix multiplication problem using sequential CPU execution, OpenMP shared-memory parallelism, MPI distributed-memory parallelism, and CUDA GPU parallelism. CUDA delivered the highest measured performance, followed by OpenMP, then MPI, with sequential execution as the baseline — while every implementation was verified against the same expected result, `C[0][0] = 4000.00`.
