---
title: "Verifier Specification"
weight: 2
---

The MAYA Verifier Specification is a complete, language-agnostic verification
procedure. Together with the election record, it enables an independent
developer to write a working verifier from scratch, in any programming
language, without access to the MAYA source code or the research paper.

## Specification Documents

<figure class="data-table simple">
<div class="table-scroll">
<table>
<thead>
<tr><th>Version</th><th>Specification</th><th>Date</th></tr>
</thead>
<tbody>
<tr><td>2.0</td><td>{{< download file="spec/maya_verifier_spec_v2.pdf" >}}</td><td>July 2026</td></tr>
</tbody>
</table>
</div>
</figure>

The specification defines the exact binary format of all data, the
deterministic derivation of public parameters, the hash-based Fiat–Shamir
transcript protocol, and the step-by-step verification algorithm.

## What the Specification Covers

- Group and scalar arithmetic for Ristretto255 (Section 3)
- Data formats and serialization (Section 4)
- Deterministic generator derivation / CRS (Section 5)
- Hash computation and Fiat–Shamir transcript (Section 6)
- Challenge vector derivation (Section 7)
- Complete verification algorithm in 6 steps (Section 8)
- IPA and eCP sub-protocols (Sections 9–10)
- Test vectors for every intermediate computation (Section 11)

## Test Vectors

Section 11 of the specification provides reference values for every
intermediate computation a verifier performs, so that each stage can be
validated independently before moving to the next. A mismatch at any stage
pinpoints the module that needs fixing.

The reference test case is a shuffle of n = 4 ciphertexts with folding factor
k = 2, recursion depth d = 2, and base-case parameter m = 1, under the
permutation π = (1, 3, 0, 2). Every value is printed as 64 hexadecimal
characters encoding 32 bytes in byte order. Both the transcript and the
prover's random-number generator were initialized with fixed seeds, so the
values are reproducible on every run.

<figure class="data-table simple">
<div class="table-scroll">
<table>
<thead>
<tr><th>Stage</th><th>What to compare</th><th>Section</th></tr>
</thead>
<tbody>
<tr><td>CRS generators</td><td>B_blinding, F, first 4 of g and h</td><td>11.5–11.6</td></tr>
<tr><td>Challenge vector e</td><td>First 4 entries</td><td>11.7</td></tr>
<tr><td>Transcript challenges</td><td>y, z, x, x', x_ipp, w_agg</td><td>11.8</td></tr>
<tr><td>Flattened weights</td><td>w_L, w_R, w_O, w_V, w_c for n = 4</td><td>11.9</td></tr>
<tr><td>Proof parsing</td><td>Proof header, IPA and eCP sub-proofs</td><td>11.10–11.12</td></tr>
<tr><td>IPA scalar expansion</td><td>s_g, s_h vectors</td><td>11.13</td></tr>
<tr><td>eCP scalar expansion</td><td>Corresponding vectors</td><td>11.14</td></tr>
<tr><td>Mega-MSM result</td><td>32 zero bytes (the group identity)</td><td>11.17</td></tr>
</tbody>
</table>
</div>
</figure>

The binary test election record is not yet published. Until it is, the
hexadecimal values in Section 11 are sufficient to construct the input by hand
and to check every stage of an implementation.

## Byte-Level Reproducibility

Two conforming implementations in different languages must produce identical
accept/reject decisions on identical input bytes. Every array length is stated
and must be verified; every scalar and group element is checked for
canonicality. This is what makes the specification, rather than any particular
codebase, the reference for what a valid MAYA proof is.
