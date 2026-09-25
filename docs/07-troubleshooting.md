# Troubleshooting

| Problem | Action |
|---|---|
| WSL command not found | From Windows PowerShell, verify WSL is installed with `wsl --status` and check installed distributions with `wsl -l -v`. |
| Ubuntu does not start | Restart WSL using `wsl --shutdown` and launch it again with `wsl`. |
| `gcc` command not found | Inside Ubuntu, run `sudo apt update` followed by `sudo apt install build-essential -y`. |
| OpenMP compilation fails | Ensure the compile command includes `-fopenmp`. |
| OpenMP uses fewer threads | Run `nproc` and `echo $OMP_NUM_THREADS`. Confirm `OMP_NUM_THREADS` is set to 8. |
| MPI ping fails | Verify all VMs use the same virtual network and that the recorded IP addresses are correct. |
| SSH asks for a password | Run `ssh-copy-id` from Master to each Worker, then test using `ssh worker1 hostname`. |
| `mpirun` cannot launch workers | Verify the hostfile names, passwordless SSH, and the presence of the executable on each Worker. |
| `nvidia-smi` fails | Verify that the NVIDIA driver is installed and that the GPU is visible to the operating system. |
| `nvcc` command not found | Verify the CUDA Toolkit installation and `PATH` configuration. |

---

# Conclusion

The experiment implements a single matrix multiplication problem using sequential CPU execution, OpenMP shared-memory parallelism, MPI distributed-memory parallelism, and CUDA GPU parallelism. The sequential implementation executed first in WSL2 and established the baseline. OpenMP then reduced execution time through CPU thread-level parallelism, MPI distributed work across four virtual machines, and CUDA delivered the highest measured performance on the NVIDIA GPU. The final comparison demonstrates the practical performance differences between the four computing models while keeping the mathematical workload and verification method unchanged.
