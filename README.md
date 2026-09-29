# dftracer-demo

A demo to run dftracer with different workloads.

## Getting Started

1. **Setup the environment**:

   ```bash
   ./setup.sh
   source ./install/bin/activate
   ```

2. **Launch Jupyter**:

   ```bash
   jupyter lab
   ```

3. **Follow the demo guides**:
   - **Quick Start**: See `QUICK_START.md` for a brief overview
   - **Complete Guide**: See `DEMO_NARRATIVE_GUIDE.md` for detailed instructions

## Analyze an Existing Trace Without Running the Benchmarks

The analyzer can run on a laptop with Python 3.10 or newer. This path does not
require the cluster modules, MPI, IOR, or DLIO:

```bash
python3 -m venv .venv
.venv/bin/python -m pip install 'git+https://github.com/llnl-asr/dfanalyzer.git@v0.1.0' 'dftracer-utils==0.0.12'
.venv/bin/python - <<'PY'
from dftracer.analyzer import init_with_hydra

dfa = init_with_hydra([
    'trace_path=output/dlio/unet3d_a100/compact',
    'cluster=none',
])
try:
    result = dfa.analyze_trace()
    dfa.output.handle_result(result)
finally:
    dfa.shutdown()
PY
```

Run these commands from the repository root. Replace `trace_path` with the
directory containing your own compacted `.pfw.gz` files. `cluster=none` keeps
this small example in one process; use a distributed cluster for large traces.

## Demo Contents

- **IOR Benchmark Demo** (`demo/ior/demo.ipynb`): Traditional I/O benchmarking with DFTracer
- **DLIO Benchmark Demo** (`demo/dlio/demo.ipynb`): Deep learning I/O pattern analysis

## What You'll Learn

- How to configure and use DFTracer for I/O profiling
- Understanding different I/O patterns in HPC workloads
- Analyzing trace data to identify performance bottlenecks
- Using DFAnalyzer for visualization and insights

## License

DFTracer Demo is distributed under the terms of the MIT license.
All new contributions must be made under this license.

See [LICENSE](LICENSE) and [NOTICE](NOTICE) for details.

SPDX-License-Identifier: MIT

LLNL-CODE-2024514 — Applied Storage Research (ASR)
