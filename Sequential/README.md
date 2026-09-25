
# Sequential Matrix Multiplication

## Objective

To implement and execute matrix multiplication sequentially using a single CPU execution flow and record the execution time.

## Problem Definition

Two matrices A and B of size 4000 × 4000 are initialized with all elements equal to 1.0.

The result is calculated as:

C = A × B

Since each element contains the sum of 4000 values of 1.0, the expected value is:

C[0][0] = 4000.00

## Environment

- Operating System: Ubuntu through WSL2
- Compiler: GCC
- Matrix Size: 4000 × 4000
- Programming Language: C
- Execution Model: Sequential CPU execution

## Source File

The source code is available in:

`matrix_sequential.c`

## Compilation

```bash
gcc -O2 matrix_sequential.c -o matrix_sequential
