# DuckDB Development Guide

## Cursor Cloud specific instructions

DuckDB is a self-contained C++ analytical database engine. There are **no external services, databases, or containers** to run. Build and test commands are in the top-level `Makefile`; see `CONTRIBUTING.md` for full details.

### Build

- **Debug build** (with ASan/UBSan): `GEN=ninja make debug` — output at `build/debug/`
- **Release build**: `GEN=ninja make release` — output at `build/release/`
- Use `CMAKE_BUILD_PARALLEL_LEVEL=4` if the build OOMs on low-memory machines.
- `ccache` is available and automatically used by the build system.

### Run

- CLI shell: `build/debug/duckdb` (or `build/release/duckdb`)
- Unit tests: `build/debug/test/unittest` (fast subset, ~1 min) or `build/release/test/unittest "*"` (all tests, ~1 hr)
- See `CONTRIBUTING.md` > Testing section for sqllogictest conventions.

### Lint / Format

- `make format-check` — checks C++ (clang-format 11.0.1), Python (black), and CMake (cmake-format) formatting.
- `make format-fix` — auto-fixes formatting.
- Requires: `pip install clang-format==11.0.1 black cmake-format`

### Non-obvious gotchas

- The default `c++` compiler on the VM is **clang 18**, which requires `libstdc++-14-dev` and `libclang-rt-18-dev` for the debug build (ASan/UBSan). These are system packages, not pip packages.
- A symlink `/usr/lib/x86_64-linux-gnu/libstdc++.so -> /usr/lib/gcc/x86_64-linux-gnu/13/libstdc++.so` may be needed if the linker cannot find `-lstdc++`.
- `clang-format`, `black`, and `cmake-format` are installed to `~/.local/bin` via pip. Ensure `$HOME/.local/bin` is on `PATH`.
- The debug build takes ~8 min on 4 cores with Ninja. Release builds are slower due to optimization passes.
