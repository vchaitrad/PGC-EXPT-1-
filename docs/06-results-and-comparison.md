# Results and Performance Comparison

All four implementations were run on the same **4000 × 4000** matrix multiplication problem and all produced the same verification value, `C[0][0] = 4000.00`.

| Implementation | Model | Resources | Time | Verification |
|---|---|---|---|---|
| Sequential | Single CPU execution | 1 CPU core | 244.120000 s | 4000.00 |
| OpenMP | Shared memory | 8 CPU threads | 30.830434 s | 4000.00 |
| MPI | Distributed memory | 4 processes / 4 VMs | 92.979510 s | 4000.00 |
| CUDA | GPU parallelism | NVIDIA RTX 4500 Ada | 0.165004 s | 4000.00 |

## Speedup Formula

```
Speedup = Sequential Execution Time / Parallel Execution Time
```

| Implementation | Execution Time | Speedup |
|---|---|---|
| Sequential | 244.120000 s | 1.00× |
| OpenMP | 30.830434 s | 7.92× |
| MPI | 92.979510 s | 2.63× |
| CUDA | 0.165004 s | 1479.48× |

## Observations

- The sequential program is the baseline because it performs the computation using one CPU execution flow.
- OpenMP reduces execution time by sharing the outer-loop iterations among eight CPU threads.
- MPI demonstrates distributed memory but introduces communication and virtual-network overhead.
- CUDA provides the highest performance for this workload by launching a large number of logical GPU threads.
- The same mathematical operation and verification value are maintained across all four implementations.

## Recommended Result Screenshots

When you actually run each part, save your terminal screenshots into the matching folder under [`screenshots/`](../screenshots):

- **`screenshots/sequential/`** — PowerShell WSL verification, `gcc --version`, source code, compilation, and final output.
- **`screenshots/openmp/`** — `nproc`, `OMP_NUM_THREADS`, source code, `htop`, and final output.
- **`screenshots/mpi/`** — four-VM network connectivity, SSH test, `mpirun` output showing ranks and final verification.
- **`screenshots/cuda/`** — `nvidia-smi`, `nvcc --version`, compilation, and final CUDA output.
