# CSCI 2951-O SAT Solver

A SAT solver implemented in Julia. Supports multiple solving strategies including DPLL and several CDCL variants which you can run in parallel. 

## Project Structure

```
.
├── src/
│   ├── main.jl                     # Entry point — parses args and dispatches to solver
│   ├── sat_instance.jl             # SAT instance data structures
│   ├── objects.jl                  # Shared objects/types
│   ├── dimacs_parser.jl            # DIMACS CNF file parser
│   ├── model_timer.jl              # Timing utilities
│   ├── check.jl                    # Solution verifier
│   └── solvers/
│       ├── dpll.jl                 # DPLL solver
│       ├── dpll_bad.jl             # Baseline/naive DPLL
│       ├── cdcl_basic_solver.jl    # CDCL with basic restart
│       ├── cdcl_vsids_solver.jl    # CDCL with VSIDS heuristic
│       └── cdcl_vsids_luby_solver.jl # CDCL with VSIDS + Luby restarts
├── input/                          # CNF input files (DIMACS format)
├── run.sh                          # Run a single input file
├── runAll.sh                       # Batch runner over an input folder
└── runJob.sh                       # SLURM job submission script
```

## Available Solvers

| Solver name         | Description                                      |
|---------------------|--------------------------------------------------|
| `dpll`              | Standard DPLL with unit propagation              |
| `dpll_bad`          | Baseline DPLL (no optimizations)                 |
| `cdcl_basic`        | CDCL with basic conflict-driven clause learning  |
| `cdcl_vsids`        | CDCL with VSIDS variable ordering heuristic      |
| `cdcl_vsids_luby`   | CDCL with VSIDS + Luby restart schedule          |

## Usage

### Single file

```bash
./run.sh --solver <solver_name> <path/to/input.cnf>
```

Example:

```bash
./run.sh --solver dpll input/example.cnf
```

### Batch run

```bash
./runAll.sh <inputFolder/> <timeLimit> <logFile> [solver]
```

- `inputFolder/` — directory of `.cnf` files
- `timeLimit` — per-instance time limit in seconds
- `logFile` — output log (must not already exist)
- `solver` — solver name (default: `dpll`)

Results are written as JSON lines to `logFile`:

```json
{"Instance": "example.cnf", "Time": "0.42", "Result": "SAT"}
```


## Requirements

- [Julia](https://julialang.org/) (managed via `juliaup`)


# Run a solver locally
./run.sh --solver dpll input/example.cnf
```
