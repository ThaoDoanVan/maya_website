---
title: "Code & Repositories"
weight: 5
---

MAYA is implemented in Rust with two curve instantiations. The source code
accompanies the research paper.

The current artifact reproduces the performance measurements reported in the paper. A public API for allowing users to shuffle their own ciphertexts is still under development. Once available, this page will be updated with the corresponding usage instructions and commands.

## Repositories

| Repository | Contents |
|---|---|
| [maya_shuffle_proof](https://github.com/uclcrypto/maya_shuffle_proof) | The code evaluated in the paper, as `maya_ristretto` (Ristretto255, curve25519-dalek) and `maya_p256` (NIST P-256, arkworks) |
| [maya_extensions](https://github.com/uclcrypto/maya_extensions) | The current implementation, together with the MAYAsk protocols |

See the [Changelog]({{< relref "changelog.md" >}}) for what changed between
them. The commands below are for `maya_shuffle_proof`.

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
