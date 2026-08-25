---
title: "Code & Repositories"
weight: 5
---

MAYA is implemented in Rust with two curve instantiations. The source code
accompanies the research paper.

## Repositories

| Repository | Curve | Library |
|-----------|-------|---------|
| maya_ristretto | Ristretto255 (Curve25519) | curve25519-dalek |
| maya_p256 | NIST P-256 | arkworks |

GitHub: [https://github.com/uclcrypto/maya_shuffle_proof](https://github.com/uclcrypto/maya_shuffle_proof)

## Requirements

- Hardware: x86-64 CPU, minimum 8 GB RAM (16 GB recommended)
- Software: Rust >= 1.81 (install via rustup.rs)

## Quick Start

```bash
cd maya_ristretto
cargo build --release --features yoloproofs,parallel
QUICK=1 cargo bench --bench r1cs --features yoloproofs,parallel
```

## Reproducing the benchmarks

```bash
cd maya_ristretto
# Single-threaded
cargo bench --bench r1cs --features yoloproofs
# Multi-threaded
cargo bench --bench r1cs --features yoloproofs,parallel
```

## Repository Structure

```
maya_ristretto/          # Primary instantiation
  src/                    # Library source
  benches/r1cs.rs         # Benchmark
maya_p256/                # Secondary instantiation
  src/                    # Library source
  benches/r1cs.rs         # Benchmark
```

## Verificatum Baseline

For comparison benchmarks against Verificatum v3.1.0 (Java 17+), see
[https://www.verificatum.org/](https://www.verificatum.org/).
