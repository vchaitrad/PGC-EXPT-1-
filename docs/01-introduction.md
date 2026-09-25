# 1. Introduction

This laboratory experiment implements the same **4000 × 4000 matrix multiplication** problem using four different computing models:

1. **Sequential** — single CPU core (baseline)
2. **OpenMP** — shared-memory, multi-threaded CPU
3. **MPI** — distributed-memory, multi-process across 4 VMs
4. **CUDA** — GPU parallelism (NVIDIA)

The experiment follows the complete workflow from environment verification to the final performance comparison. For the sequential and OpenMP implementations, Windows PowerShell is used to access an installed WSL2 Ubuntu environment; the C program itself is compiled and executed inside Ubuntu.

## Execution Flow

```
Windows PowerShell
        │
        ▼
    WSL2 Ubuntu
        │
        ▼
Sequential CPU Baseline
        │
        ▼
 OpenMP Shared Memory
        │
        ▼
 MPI Distributed Memory
        │
        ▼
  CUDA GPU Parallelism
        │
        ▼
Results & Speedup Comparison
```

The reference implementation initializes matrices **A** and **B** so that every element is `1.0`. Because each output element is the sum of 4000 products of `1.0 × 1.0`, every output element of **C** is expected to be `4000.00`. This value is used across all four implementations as a correctness check.

## 2. Components Required

### Hardware
- Windows 10/11 host system
- Sufficient CPU cores and RAM for WSL and virtual machines
- Four Ubuntu virtual machines (for the MPI experiment)
- NVIDIA GPU (for the CUDA experiment)

### Software
- Windows PowerShell
- WSL2 with Ubuntu (sequential and OpenMP experiments)
- GCC / `build-essential`
- OpenMP support (bundled with GCC)
- Open MPI and OpenSSH (distributed experiment)
- CUDA Toolkit and `nvcc` (GPU experiment)
- VMware Workstation or equivalent virtualization software (MPI nodes)

### Problem Definition

| Term | Value |
|---|---|
| Matrix A | 4000 × 4000, all elements = 1.0 |
| Matrix B | 4000 × 4000, all elements = 1.0 |
| C | A × B |
| C[0][0] | 1×1 + 1×1 + ... + 1×1 (4000 terms) = **4000.00** |
