# Part B — OpenMP Matrix Multiplication

Source: [`openmp/matrix_openmp.c`](../openmp/matrix_openmp.c)

OpenMP uses multiple threads on the same shared-memory machine. The matrix multiplication algorithm remains the same, but the outer loop is divided across multiple CPU threads.

## 4.1 Prerequisites
- The WSL2 Ubuntu environment from Part A is working
- GCC is installed
- The WSL environment exposes multiple logical CPUs
- The sequential baseline has already been recorded

## 4.2 OpenMP Setup

```bash
wsl                         # from PowerShell, enter Ubuntu
nproc                       # check number of logical CPUs (reference: 8)
export OMP_NUM_THREADS=8    # request 8 OpenMP threads
echo $OMP_NUM_THREADS       # verify -> expected output: 8

mkdir -p ~/parallel_lab/openmp
cd ~/parallel_lab/openmp
nano matrix_openmp.c        # paste in matrix_openmp.c, save (Ctrl+O, Enter, Ctrl+X)
```

## 4.3 Source Code, Compilation and Execution

```bash
gcc -O2 -fopenmp matrix_openmp.c -o matrix_openmp
./matrix_openmp
```

- The `-fopenmp` flag enables OpenMP directives and links the OpenMP runtime.
- While running, monitor CPU utilization in another terminal with `htop` to visually confirm multiple logical CPUs are active.

## 4.4 OpenMP Result

```
OpenMP Matrix Multiplication Completed
Matrix Size = 4000 x 4000
Number of Threads Used = 8
Execution Time = 30.830434 seconds
Verification C[0][0] = 4000.00
```

**Recorded execution time:** `30.830434` seconds
**Speedup over sequential:** `244.12 / 30.830434 = 7.92×`
**Why it is faster:** Eight CPU threads work on different outer-loop iterations while sharing matrices A, B, and C.
