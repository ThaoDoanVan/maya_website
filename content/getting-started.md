---
title: "Getting Started: Build Your Own Verifier"
weight: 4
---

The MAYA verifier can be built and tested incrementally. Each step below
produces a testable output that can be validated against the test vectors
in the specification before moving to the next. You can implement the
verifier in any programming language.

## Where the Proof Comes From

A verifier reads two inputs:

1. The **input ciphertexts file**, taken from the bulletin board or from the
   previous mix layer. It carries the number of ciphertexts n, the two
   coordinate vectors of the input ciphertexts, and the public key (g, f, h).
2. The **proof bundle**, produced by the mix server that performed the
   shuffle. It carries the serialized proof, the vector commitment V, and the
   coordinate vectors of the output ciphertexts.

Both files are plain binary with the layout fixed in Section 4 of the
specification, and both must have exactly the length implied by their declared
counters. An implementation may read the same fields from a database or a
network API instead, as long as every field is present and every structural
check is applied.

To generate a bundle to test against, run the MAYA prover from the
[reference implementation](https://github.com/uclcrypto/maya_shuffle_proof).
Nothing about the proof is tied to that implementation or to Rust: the verification procedure is fully deterministic,
so any two conforming verifiers, in any two languages, return the same
accept/reject decision on the same input bytes.

## Prerequisites

1. The [Verifier Specification]({{< relref "specification.md" >}}) document
2. The test vectors in Section 11 of the specification
3. A Ristretto255 library for your language:
   - Rust: [curve25519-dalek](https://github.com/dalek-cryptography/curve25519-dalek)
   - Go: [filippo.io/edwards25519](https://pkg.go.dev/filippo.io/edwards25519)
   - Python: [pysodium](https://github.com/stef/pysodium) (libsodium bindings)
   - JavaScript: ristretto255-js
   - C: libsodium (Ristretto255 support since v1.0.18)
4. An HMAC-SHA-256 implementation. This is the only hash primitive the
   specification uses, and it is available in the standard libraries of C
   (OpenSSL), Python, Go, Java 9+, and Rust.

## Implementation Steps

### Step 1: Scalar and Point I/O (Spec Sections 3, 4.1)

- Implement scalar encoding/decoding, point compression/decompression, and
  canonicality checks for Ristretto255.
- Test: Decode the test-vector scalars and points; confirm round-trip encoding.

### Step 2: CRS Generators (Spec Section 5)

- Implement HMAC-SHA-256 in counter mode, the FromUniformBytes hash-to-group
  map, and the Pedersen/Bulletproof generator derivation.
- Test: Compare B_blinding, F, and the first 4 entries of g and h against
  the test vectors in Section 11.5-11.6.

### Step 3: HMAC Transcript (Spec Section 6)

- Implement the HMAC-SHA-256 transcript state machine (init, append, challenge).
- Test: Replay the test-vector transcript and compare each derived challenge
  (y, z, x, ...) against Section 11.8.

### Step 4: Challenge Vector e (Spec Section 7)

- Implement the dedicated transcript and counter-mode HMAC expansion.
- Test: Compare the first 4 entries of e against Section 11.7.

### Step 5: Election Record Parser (Spec Section 4)

- Parse the input ciphertexts file and proof bundle.
- Run all structural validation checks (file lengths, n >= 2, k in
  {2,3,4,5,6,8,16}, point decompression, scalar canonicality).
- Test: Successfully parse the n=4 test election record.

### Step 6: Constraint Flattening (Spec Section 8.3)

- Build the permutation circuit and compute the weight vectors
  w_L, w_R, w_O, w_V, w_c.
- Test: Compare flattened weights against Section 11.9.

### Step 7: IPA Scalar Expansion (Spec Section 9)

- Implement the IPA verification-scalars function: replay the IPA
  sub-transcript, expand the final witness back to n dimensions.
- Test: Compare against Section 11.13.

### Step 8: eCP Scalar Expansion (Spec Section 10)

- Same as Step 7 for the eCP sub-protocol.
- Test: Compare against Section 11.14.

### Step 9: Mega-MSM Assembly (Spec Sections 8.5-8.6)

- Compute all batched scalars, assemble the point/scalar vectors, execute
  the MSM, and check identity. A variable-time MSM is fine here: every input
  to the verifier is public.
- Test: The Mega-MSM result must be 32 zero bytes (the Ristretto255
  identity). If so, the proof is Accepted.

## A Note on What the Verifier Must Not Trust

Every byte of the election record and the proof is treated as potentially
adversarial. The verifier must recompute the CRS generators and all challenges
itself, and must never accept generators, challenge vectors, or challenge
scalars supplied in the input data. This is also why the specification
mandates explicit bounds checks, canonical scalar checks, and decompression
checks on every field.

## What Comes Next

Once your verifier accepts the n=4 test case, we recommend trying it against
larger test cases (n=100, n=1000) and against malformed proofs, which exercise
the rejection paths that the n=4 case never reaches. If you complete an
independent verifier, we would like to hear about it — open an issue on the
repository.
