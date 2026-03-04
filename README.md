# CSC 5760 — MPI Homework #1
**Tennessee Tech University | Spring 2026**
**Instructor: Anthony Skjellum**

---

## Overview

MPI programming assignment covering point-to-point communication, collective operations, communicator splitting, and a 2D parallel implementation of Conway's Game of Life.

---

## Files

| File | Problem | Description |
|------|---------|-------------|
| `hw_1_1.cpp` | 1 | Ring topology — hand-coded |
| `hw_1_2.cpp` | 2 | Ring topology — LLM-generated (vibe coding) |
| `hw_1_3.cpp` | 3 | Parallel dot product with tree reduction — hand-coded |
| `hw_1_4.cpp` | 4 | Parallel dot product — LLM-generated (vibe coding) |
| `hw_1_5.cpp` | 5 | MPI communicator splitting |
| `hw_1_6.cpp` | 6a | `MPI_Allreduce` via `MPI_Reduce` + `MPI_Bcast` |
| `hw_1_6_2.cpp` | 6a | `MPI_Allreduce` direct |
| `hw_1_6_3.cpp` | 6b | `MPI_Allgather` via `MPI_Gather` + `MPI_Bcast` |
| `hw_1_6_4.cpp` | 6b | `MPI_Allgather` direct |
| `hw_1_6_5.cpp` | 6c | `MPI_Alltoall` personalized communication |
| `hw_1_7.cpp` | G1 | Conway's Game of Life — 2D MPI decomposition |

---

## Build

All programs compile with `mpicxx` (C++) or `mpicc` (C).

```bash
mpicxx -o hw_1_1 hw_1_1.cpp
mpicxx -o hw_1_2 hw_1_2.cpp
mpicxx -o hw_1_3 hw_1_3.cpp
mpicc  -O2 -o hw_1_4 hw_1_4.cpp -lm
mpicxx -o hw_1_5 hw_1_5.cpp
mpicxx -o hw_1_6   hw_1_6.cpp
mpicxx -o hw_1_6_2 hw_1_6_2.cpp
mpicxx -o hw_1_6_3 hw_1_6_3.cpp
mpicxx -o hw_1_6_4 hw_1_6_4.cpp
mpicxx -o hw_1_6_5 hw_1_6_5.cpp
mpicxx -o hw_1_7 hw_1_7.cpp
```

---

## Usage

### Problem 1 — Ring Topology (hand-coded)
N is hardcoded at compile time (default N=5).
```bash
mpirun -n 4 ./hw_1_1
```

### Problem 2 — Ring Topology (LLM)
Prompts for N at runtime. Hardcoded to 4 processes.
```bash
mpirun -n 4 ./hw_1_2
```

### Problem 3 — Dot Product (hand-coded)
N is set in source (default N=4096). Run with P = 1, 2, or 4.
```bash
mpirun -n 1 ./hw_1_3
mpirun -n 2 ./hw_1_3
mpirun -n 4 ./hw_1_3
```

### Problem 4 — Dot Product (LLM)
N passed as command-line argument.
```bash
mpirun -n 4 ./hw_1_4 4096
```

### Problem 5 — Communicator Splitting
Requires exactly 6 processes (P=2, Q=3 hardcoded).
```bash
mpirun -n 6 ./hw_1_5
```

### Problem 6 — Collective Operations
Requires at least 4 processes.
```bash
mpirun -n 4 ./hw_1_6
mpirun -n 4 ./hw_1_6_2
mpirun -n 4 ./hw_1_6_3
mpirun -n 4 ./hw_1_6_4
mpirun -n 4 ./hw_1_6_5
```

### Problem G1 — Game of Life
```
mpirun -n <P*Q> ./hw_1_7 P Q N iterations
```
Constraints: `P*Q` must equal number of processes; `N` must be divisible by both `P` and `Q`.

```bash
# 2x2 grid, 32x32 board, 200 iterations
mpirun -n 4 ./hw_1_7 2 2 32 200

# 4x1 grid, 32x32 board, 200 iterations
mpirun -n 4 ./hw_1_7 4 1 32 200

# 2x4 grid, 512x512 board, 10 iterations (8 process minimum)
mpirun -n 8 ./hw_1_7 2 4 512 10

# Single process, no inter-process communication
mpirun -n 1 ./hw_1_7 1 1 32 5
```

---

## Notes

- Problems 1 and 3 were written by hand with no LLM assistance per assignment requirements.
- Problems 2 and 4 were generated via vibe coding sessions with Claude Sonnet 4.6 (claude.ai) and are documented in the submitted report.
- The `print_full_grid()` function in `hw_1_7.cpp` was LLM-generated (Claude Sonnet 4.6, 2026-03-04); the remainder of that file was written by hand.
