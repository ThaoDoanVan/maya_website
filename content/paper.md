---
title: "Paper"
weight: 1
---

# MAYA: A Short Shuffle Argument With Fast Verification

**Authors:** Thi Van Thao Doan, Olivier Pereira, and Thomas Peters

**Affiliation:** UCLouvain, B-1348 Louvain-la-Neuve, Belgium

**Published at:** 33rd ACM Conference on Computer and Communications Security
([CCS'26](https://www.sigsac.org/ccs/CCS2026/call-for/call-for-papers.html)),
to appear.

## Abstract

Shuffle arguments dominate the audit data in end-to-end verifiable elections
when homomorphic tallying is not applicable. Deployed mixnets in countries
like Australia, Estonia, Israel, Norway, Spain, and Switzerland produce
proofs that scale linearly, reaching gigabytes for millions of ciphertexts.
MAYA achieves O(log n) communication complexity with a transparent setup
requiring no trusted parameters. For one million ciphertexts, MAYA produces
proofs that are 0.002% the size of Verificatum's, with comparable prover
time and 9% of the verification time.

## Download

{{< download url="https://eprint.iacr.org/2026/941.pdf" label="Download PDF" >}}

## Citation

```bibtex
@inproceedings{maya2026,
  title     = {MAYA: A Short Shuffle Argument With Fast Verification},
  author    = {Doan, Thi Van Thao and Pereira, Olivier and Peters, Thomas},
  booktitle = {Proceedings of the 2026 ACM SIGSAC Conference on Computer
               and Communications Security (CCS '26)},
  publisher = {ACM},
  year      = {2026}
}
```

## Key Contributions

1. Computational parameter optimization (k=4 folding factor)
2. Iterative padding with bounded overhead
3. Aggregated argument batching (1.5x reduction in recursive prover time)
4. Bridged consistency for aggregated commitments
5. Production-scale evaluation on Ristretto255 and P-256
