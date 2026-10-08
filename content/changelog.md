---
title: "Changelog"
weight: 6
---

Two versions of the MAYA implementation are public.

## Version 2 — current (August 2026)

[uclcrypto/maya_extensions](https://github.com/uclcrypto/maya_extensions)

**One hash function.** Version 1 used three primitives: a Merlin transcript for
Fiat–Shamir, SHA3-512 for the Pedersen generators, and SHAKE256 for the
Bulletproof generators. Version 2 uses HMAC-SHA-256 for all three. HMAC-SHA-256
is in the standard library of every mainstream language, so a verifier can be
written in any of them without reproducing a Rust-specific transcript
construction.

**Extensions.** This repository also contains the five
[MAYAsk]({{< relref "mayask.md" >}}) protocols. Plain MAYA is
&Pi;<sub>1</sub> with a single block.

**Benchmarks.** The performance table on the
[home page]({{< relref "_index.md" >}}#performance) was produced by this
repository, by the &Pi;<sub>1</sub> benchmark in `pi1_pi2_pi3/benches/pi1.rs`.
Each configuration is a triple `(k, B, b)`: folding factor, number of blocks,
block size. The table uses a single block, so `b` is the number of ciphertexts.
The configurations are set in the `configs` list in `custom_benchmark`:

```
(4, 1, 1_000), (4, 1, 10_000), (4, 1, 100_000), (4, 1, 1_000_000)
```

```bash
cd pi1_pi2_pi3
cargo bench --bench pi1 --features yoloproofs,parallel   # multi-threaded
cargo bench --bench pi1 --features yoloproofs            # single-threaded
```

## Version 1 — CCS '26 artifact (May 2026)

[uclcrypto/maya_shuffle_proof](https://github.com/uclcrypto/maya_shuffle_proof)

The code evaluated in the [Paper]({{< relref "paper.md" >}}). It reproduces the
measurements reported there. Ristretto255 and P-256.

## Which version to use

- Reproducing the paper's tables: version 1.
- Blocks, multiple keys, or decryption mixnets: version 2.
