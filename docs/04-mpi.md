# Part C — MPI Distributed Matrix Multiplication

Source: [`mpi/matrix_mpi.c`](../mpi/matrix_mpi.c) · Hostfile: [`mpi/hosts`](../mpi/hosts)

MPI uses multiple independent processes. One Master VM and three Worker VMs are connected through the same virtual network. Each process has its own memory space, so data must be explicitly communicated between processes.

## 5.1 Prerequisites
- VMware Workstation or an equivalent virtualization platform
- Four Ubuntu virtual machines: one Master, three Workers
- All four VMs connected to the same virtual network
- OpenSSH Server and Open MPI installed on all nodes
- Passwordless SSH from the Master to the Workers

## 5.2 VM Cluster Setup

| Node | Hostname | IP Address | MPI Role |
|---|---|---|---|
| Master | `master` | `192.168.125.128` | Rank 0 |
| Worker1 | `worker1` | `192.168.125.129` | Rank 1 |
| Worker2 | `worker2` | `192.168.125.130` | Rank 2 |
| Worker3 | `worker3` | `192.168.125.131` | Rank 3 |

**Matrix distribution:** Rank 0/1/2/3 → 1000 rows each.

```bash
# On each VM, set a unique hostname
sudo hostnamectl set-hostname master     # (worker1 / worker2 / worker3 on the others)

# On each VM
hostname -I                              # record the IP address

# From Master — verify connectivity
ping -c 4 192.168.125.129
ping -c 4 192.168.125.130
ping -c 4 192.168.125.131
```

## 5.3 OpenSSH and Open MPI (on every VM)

```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable --now ssh

sudo apt install openmpi-bin libopenmpi-dev -y

mpicc --version
mpirun --version
```

### Passwordless SSH (from Master)

```bash
ssh-keygen -t rsa
ssh-copy-id worker1
ssh-copy-id worker2
ssh-copy-id worker3

# verify
ssh worker1 hostname
ssh worker2 hostname
ssh worker3 hostname
```

## 5.4 Hostfile and MPI Program

```bash
mkdir -p ~/parallel_lab/mpi
cd ~/parallel_lab/mpi
nano hosts
```

Contents of `hosts` (see [`mpi/hosts`](../mpi/hosts)):
```
master slots=1
worker1 slots=1
worker2 slots=1
worker3 slots=1
```

Then create `matrix_mpi.c` (see [`mpi/matrix_mpi.c`](../mpi/matrix_mpi.c)) with `nano matrix_mpi.c`.

**Data flow:**
```
Matrix A (4000 rows)
        │
   MPI_Scatter
        │
 ┌──────┼──────┬──────┐
 ▼      ▼      ▼      ▼
Rank0  Rank1  Rank2  Rank3   (1000 rows each)

Matrix B ── MPI_Bcast ──▶ all ranks

Each rank computes local_C
        │
   MPI_Gather
        │
        ▼
Complete C on Rank 0
```

## 5.5 Compilation, Remote Copy and Execution

```bash
mpicc -O2 matrix_mpi.c -o matrix_mpi

scp matrix_mpi worker1:~/matrix_mpi
scp matrix_mpi worker2:~/matrix_mpi
scp matrix_mpi worker3:~/matrix_mpi

mpirun -np 4 --hostfile hosts sh -c '$HOME/matrix_mpi'
```

> **Note:** If your Open MPI build refuses root execution, run as a normal user. Do not disable Open MPI safety checks unless required by your controlled lab environment.

## 5.6 MPI Result

```
MPI Matrix Multiplication Completed
Matrix Size = 4000 x 4000
Number of MPI Processes = 4
Execution Time = 92.979510 seconds
Verification C[0][0] = 4000.00
```

**Recorded execution time:** `92.979510` seconds
**Speedup over sequential:** `244.12 / 92.979510 = 2.63×`
**Why communication matters:** MPI must distribute input data and gather partial results across separate address spaces and network-connected VMs.
