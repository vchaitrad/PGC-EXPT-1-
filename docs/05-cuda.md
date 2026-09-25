# Part D — CUDA Matrix Multiplication

Source: [`cuda/matrix_cuda.cu`](../cuda/matrix_cuda.cu)

CUDA offloads the matrix multiplication to an NVIDIA GPU. The CPU prepares the matrices and transfers them to GPU memory. A CUDA kernel is then launched using a grid of blocks, and each logical thread computes one output element of matrix C.

## 6.1 Prerequisites
- NVIDIA CUDA-capable GPU
- NVIDIA driver installed and GPU recognized
- CUDA Toolkit installed, `nvcc` compiler available
- Supported C/C++ host compiler
- Sufficient GPU memory for the matrix data

## 6.2 GPU and CUDA Verification

```bash
nvidia-smi        # confirms driver + GPU (reference: NVIDIA RTX 4500 Ada Generation)
nvcc --version     # confirms CUDA Toolkit installation

mkdir -p ~/parallel_lab/cuda
cd ~/parallel_lab/cuda
nano matrix_cuda.cu    # paste in matrix_cuda.cu, save (Ctrl+O, Enter, Ctrl+X)
```

## 6.3 CUDA Program, Compilation and Execution

```bash
nvcc -O2 matrix_cuda.cu -o matrix_cuda
./matrix_cuda
```

The CPU allocates memory, transfers A and B to GPU memory, launches the kernel, transfers C back to the CPU, and verifies the output.

### Execution Configuration

| Parameter | Configuration |
|---|---|
| Matrix size | 4000 × 4000 |
| Block size | 16 × 16 = 256 threads/block |
| Grid size | 250 × 250 blocks |
| Total blocks | 62,500 |
| Logical CUDA threads | 16,000,000 |

Because `4000 / 16 = 250`, the grid is `250 × 250` blocks. Each block contains 256 threads, giving `62,500 × 256 = 16,000,000` logical thread instances — corresponding conceptually to the 4000 × 4000 output elements.

## 6.4 CUDA Result

```
CUDA Matrix Multiplication Completed
Matrix Size = 4000 x 4000
Grid Size = 250 x 250 blocks
Block Size = 16 x 16 threads
Kernel Execution Time = 0.146443 seconds
Total CUDA Phase Time = 0.165004 seconds
Verification C[0][0] = 4000.00
```

**Recorded total CUDA phase time:** `0.165004` seconds
**Recorded kernel-only time:** `0.146443` seconds
**Speedup over sequential (using total CUDA phase):** `244.12 / 0.165004 = 1479.48×`

> **Important:** Total CUDA phase time includes host-to-device transfer, kernel execution, and device-to-host transfer.
