# Part A — Sequential Matrix Multiplication

Source: [`sequential/matrix_sequential.c`](../sequential/matrix_sequential.c)

The sequential implementation is performed first because it provides the baseline execution time against which OpenMP, MPI, and CUDA are compared.

## 3.1 Prerequisites
- Windows PowerShell is available
- WSL2 is installed and an Ubuntu distribution is available
- Internet access is available for package installation inside Ubuntu
- The user has permission to run `sudo` commands in Ubuntu

## 3.2 Windows PowerShell and WSL Verification

| Step | Where | Command | Why / Expected Result |
|---|---|---|---|
| 1. Open Windows PowerShell | Windows host | — | Use only for WSL verification and launching Ubuntu |
| 2. Verify WSL is installed | PowerShell | `wsl --status` | Displays WSL status/config; WSL2 preferred |
| 3. List installed distributions | PowerShell | `wsl -l -v` | `VERSION` column should show `2` for WSL2 |
| 4. Start Ubuntu | PowerShell | `wsl` | Prompt changes to `user@computer:~$` |

## 3.3 Ubuntu/GCC Setup inside WSL

| Step | Command | Why |
|---|---|---|
| 5. Update package index | `sudo apt update` | Refreshes package index |
| 6. Install build tools | `sudo apt install build-essential -y` | Installs GCC, make, and standard libraries |
| 7. Verify GCC | `gcc --version` | Confirms compiler is available |

## 3.4 Working Directory and Source Code

```bash
mkdir -p ~/parallel_lab/sequential
cd ~/parallel_lab/sequential
nano matrix_sequential.c
```

Paste in the contents of [`matrix_sequential.c`](../sequential/matrix_sequential.c). Save with `Ctrl+O`, `Enter`, then exit with `Ctrl+X`.

## 3.5 Compilation and Execution

```bash
gcc -O2 matrix_sequential.c -o matrix_sequential
ls -l                    # confirm the executable was created
./matrix_sequential
```

- **Compilation:** `-O2` enables standard compiler optimizations; no errors expected.
- **Execution:** performs the full sequential multiplication and reports completion, execution time, and `C[0][0]`.

## 3.6 Sequential Result

```
Initializing 4000 x 4000 matrices...
Sequential Matrix Multiplication Completed
Matrix Size = 4000 x 4000
Execution Time = 244.120000 seconds
Verification C[0][0] = 4000.00
```

**Recorded execution time:** `244.120000` seconds
**Verification:** `C[0][0] = 4000.00`

This sequential time becomes the baseline for all speedup calculations.
