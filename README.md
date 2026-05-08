# Hospital Patient Triage & Bed Allocator
**CL2006 – Operating Systems Lab | FAST-NUCES CFD | Spring 2026**

---

## Group Members
| # | Name | Roll No |
|---|------|---------|
| 1 | Member 1 | 23F-0690|
| 2 | Member 2 | 23f-0674 |
| 3 | Member 3 | 24F-0704 |

---

## OS Concepts Demonstrated
- **Process Management** — `fork()` + `execv()` per patient, `SIGCHLD` + `waitpid(WNOHANG)` zombie reaping
- **IPC** — Anonymous pipe (triage → admissions), Named FIFO (discharge signal), Shared Memory (bed bitmap)
- **CPU Scheduling** — Priority Queue (min-heap), FCFS, SJF, Priority, Round Robin simulation
- **Threads** — Receptionist, Scheduler, Nurse threads (POSIX pthreads)
- **Synchronization** — `pthread_mutex_t`, `pthread_cond_t`, `sem_t` (counting semaphores)
- **Memory Management** — Best-Fit / First-Fit / Worst-Fit, coalescing, fragmentation reporting, paging simulation

---

## Project Structure
```
hospital_project/
├── src/
│   ├── hospital.h          # Shared structs, constants, prototypes
│   ├── admissions.c        # Main process — threads, IPC, scheduling
│   ├── patient_simulator.c # Child process — treatment simulation
│   ├── bed_allocator.c     # Memory management — Best/First/Worst Fit
│   └── scheduler.c         # Priority queue + scheduling algorithms
├── scripts/
│   ├── triage.sh           # Patient input, validation, priority mapping
│   ├── start_hospital.sh   # Launch hospital system
│   ├── stop_hospital.sh    # Graceful shutdown + cleanup
│   └── stress_test.sh      # 20-patient automated stress test
├── logs/
│   ├── schedule_log.txt    # Scheduling simulation output
│   └── memory_log.txt      # Fragmentation event log
├── Makefile
└── README.md
```

---

## Dependencies
- Linux (Ubuntu 20.04+ recommended)
- GCC with pthreads support (`gcc -lpthread`)
- Python 3 (used by `triage.sh` to pack binary struct)
- `make`, `valgrind` (for testing)

---

## Build Instructions
```bash
# 1. Build all binaries (zero warnings required)
make all

# 2. Start the hospital (default: Best-Fit strategy)
make run

# OR with a specific strategy:
bash scripts/start_hospital.sh --strategy first
bash scripts/start_hospital.sh --strategy worst
```

---

## How to Run

### Admit a Patient
```bash
# Basic: ./scripts/triage.sh <name> <age> <severity 1-10>
./scripts/triage.sh Ahmed_Khan 35 8

# Infectious patient (goes to Isolation bed):
./scripts/triage.sh Sara_Ali 22 5 --infectious

# Critical patient (severity 9-10 → ICU):
./scripts/triage.sh Ali_Hassan 55 10
```

### Stop the Hospital
```bash
make stop   # OR
bash scripts/stop_hospital.sh
```

### Run Stress Test (20 patients)
```bash
make test
```

### Check Memory Leaks
```bash
make valgrind
```

---

## Scheduling Strategies
| Flag | Algorithm |
|------|-----------|
| `--strategy best` | Best-Fit (default) |
| `--strategy first` | First-Fit |
| `--strategy worst` | Worst-Fit |

---

## Triage Priority Mapping
| Severity | Priority | Bed Type |
|----------|----------|----------|
| 9–10 | 1 (Critical) | ICU |
| 7–8  | 2 (Very Urgent) | ICU |
| 5–6  | 3 (Urgent) | General / Isolation |
| 3–4  | 4 (Less Urgent) | General |
| 1–2  | 5 (Non-Urgent) | General |

---

## Log Files
- `logs/schedule_log.txt` — FCFS, SJF, Priority, Round Robin simulation output with average wait/TAT times
- `logs/memory_log.txt` — Timestamped fragmentation stats after every allocation/deallocation event
# Hospital-Patient-Triage-Bed-Allocator-Using-Operating-Systems-Concepts
