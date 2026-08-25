---
title: "FAQ"
weight: 7
---

### What is MAYA?

MAYA is a shuffle argument with O(log n) communication complexity and a
transparent setup. It proves that a mix server correctly permuted and
re-randomized a list of ElGamal ciphertexts without altering, adding,
or dropping any vote.

### What curves does MAYA support?

MAYA is instantiated on two curves: Ristretto255 (Curve25519, via
curve25519-dalek) and NIST P-256 (via arkworks). The protocol is
algebraically generic and can be adapted to other prime-order groups.

### What is the trust model?

MAYA uses a transparent (public-coin) setup requiring no trusted parameters.
The only inputs taken on faith are the group definition and the
specification itself.

### Can MAYA handle ciphertexts that carry more than one encrypted value?

Yes. Some elections encrypt a whole vector of values under one randomness and
several public keys at once, so that a single ciphertext is a wide row rather
than a pair of group elements. MAYA generalizes to that setting: the
permutation part of the argument is untouched, and only the part that ties the
shuffled ciphertexts back to the permutation has to account for the extra
columns. The prover sends a few more group elements up front, one per extra
column, but the recursive part of the argument does not grow at all, so the
proof stays logarithmic in the number of ciphertexts. See the
[Paper]({{< relref "paper.md" >}}) for the construction.

### Can MAYA handle non-power-of-2 ciphertext counts?

Yes. MAYA uses iterative padding, which pads incrementally at each recursion
level to a multiple of k, bounding total overhead to O(log_k n) additional
elements regardless of how n aligns with powers of k.

### What folding factor should I use?

k=4 is the recommended sweet spot. It reduces prover time by ~10% compared
to binary folding (k=2) by halving recursion depth while avoiding the
arithmetic overhead of larger radices (k >= 8).

### Does checking a proof require Rust, or any particular language?

No. A MAYA proof is a sequence of bytes, and verification is a deterministic
computation over those bytes. The specification fixes the binary formats, the
generator derivation, and the Fiat-Shamir transcript exactly, so any two
conforming verifiers — whatever language they are written in — return the same
accept/reject decision on the same input. The reference implementation happens
to be Rust; nothing in the proof depends on that.

### Can I check a MAYA proof myself?

Yes, and that is a primary design goal. Checking a proof means running a
verifier over the published election record; anyone can run one, and anyone
can build one. You do not have to trust the software we wrote: the Verifier
Specification describes the whole procedure in plain, language-agnostic terms,
with test vectors at every step, so an independent developer can write a
verifier from scratch and you can compare its verdict against ours. The
[Getting Started]({{< relref "getting-started.md" >}}) page walks through that, one testable
step at a time.

### What is out of scope for the MAYA verifier?

The verifier does not cover: ElGamal key generation, plaintext vote
validity, decryption of the final ciphertext list, or bulletin board
consistency. A MAYA proof only shows that the output is a correct
shuffle of the input for the given public key.
