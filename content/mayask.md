---
title: "MAYAsk: Swiss-Knife Extensions"
weight: 3
---

Real-world election systems rarely fit the standard cryptographic model of a
single list of ciphertexts encrypted under one public key. In practice,
ciphertexts may need to remain grouped by precinct or municipality, different
groups may use different encryption keys, and shuffling may be combined with
partial decryption or secret exponentiation. Such requirements arise, for
example, in the Swiss Post e-voting system
([Cortier et al., 2025](https://eprint.iacr.org/2025/1625)) and other recent
voting constructions.

**MAYAsk** — MAYA Swiss-Knife — is the extended construction that accommodates
these settings while preserving the properties of the base protocol.
Specifically, it gives logarithmic-size arguments for block-respecting
shuffles, shuffles of ciphertexts encrypted under different public keys,
shuffles combined with partial decryption using a mixer's secret-key share,
and shuffles combined with secret exponentiation of selected ciphertext
components. In all cases the resulting arguments retain logarithmic size in
the number of ciphertexts, require a transparent setup, and have verification
dominated by a single multi-scalar multiplication.

## Extensions

**Block permutations.** Proves that ballots are shuffled strictly within their
designated blocks — municipalities, precincts, or ballot styles — without
crossing boundaries. Blocks may be uniform or of differing sizes.

**Multiple keys.** Supports federated settings where different districts
encrypt ballots under entirely independent public keys.

**Shuffle-and-exponentiate.** Allows a mixer to apply a publicly anchored
secret exponentiation during the shuffle, as required by oblivious PRF
evaluation, plaintext-equivalence tests, and the `DbleMixExp` operation of
recent Swiss Post proposals.

**Decryption mixnets.** Enables each mix server to verifiably strip a layer of
encryption while shuffling, removing the need for a separate decryption phase.
Threshold key sharing is fully supported.

All four preserve MAYA's $O(\log n)$ proof size. Adding block structures or
multiple keys incurs essentially zero overhead compared to the base protocol;
the decryption mixnet adds about 7% to multi-threaded prover time, and
shuffle-and-exponentiate carries a moderate overhead from its extra
exponentiation and doubled ciphertext count.

> The paper is not yet publicly available. It will be linked here once it is.

## Performance

<figure class="data-table simple">
<div class="table-scroll">
<table>
<thead>
<tr><th>Scenario</th><th>n</th><th>Prover (s)</th><th>Verifier (s)</th></tr>
</thead>
<tbody>
<tr class="group"><td colspan="4">&Pi;<sub>1</sub>: Uniform blocks, single key</td></tr>
<tr><td>1k &times; 20</td><td>20k</td><td>1.3 / 3.9</td><td>0.33 / 0.97</td></tr>
<tr><td>1k &times; 80</td><td>80k</td><td>5.6 / 15.4</td><td>1.3 / 3.9</td></tr>
<tr><td>1k &times; 320</td><td>320k</td><td>22.2 / 62.3</td><td>5.1 / 14.5</td></tr>
<tr><td>10k &times; 20</td><td>200k</td><td>13.9 / 38.4</td><td>3.2 / 9.7</td></tr>
<tr><td>1 &times; 200k (MAYA)</td><td>200k</td><td>13.2 / 39.7</td><td>3.1 / 9.1</td></tr>
<tr><td>10k &times; 80</td><td>800k</td><td>55.5 / 165.8</td><td>12.7 / 38.8</td></tr>
<tr><td>100k &times; 20</td><td>2M</td><td>138.3 / 391.5</td><td>31.5 / 97.1</td></tr>
<tr class="group"><td colspan="4">&Pi;<sub>2</sub>: Heterogeneous blocks, single key</td></tr>
<tr><td>1k &times; [50, 150]</td><td>200k</td><td>13.5 / 44.4</td><td>3.1 / 9.1</td></tr>
<tr><td>20k &times; [2, 3, 5]</td><td>200k</td><td>13.7 / 44.4</td><td>3.1 / 9.2</td></tr>
<tr><td>80k &times; [3, 7]</td><td>800k</td><td>54.8 / 159.7</td><td>12.6 / 39.7</td></tr>
<tr class="group"><td colspan="4">&Pi;<sub>3</sub>: Multiple keys</td></tr>
<tr><td>Key 1: 1k &times; [37, 63]<br>Key 2: 500 &times; [149, 14, 37]</td><td>200k</td><td>13.2 / 38.6</td><td>3.2 / 10.4</td></tr>
<tr><td>Key 1: 10k &times; [5]<br>Key 2: 3.5k &times; [3, 7, 10]<br>Key 3: 20k &times; [2, 2]</td><td>200k</td><td>13.7 / 40.23</td><td>3.1 / 9.75</td></tr>
<tr><td>Keys 1&ndash;100: 1k &times; [2]</td><td>200k</td><td>13.6 / 40.19</td><td>2.3 / 9.68</td></tr>
<tr class="group"><td colspan="4">&Pi;<sub>4</sub>: Decryption mixnet (one mixer)</td></tr>
<tr><td>10k &times; 20</td><td>200k</td><td>14.9 / 45.0</td><td>3.1 / 9.6</td></tr>
<tr class="group"><td colspan="4">&Pi;<sub>5</sub>: Shuffle-and-exponentiate (DbleMixExp)</td></tr>
<tr><td>5k &times; 20</td><td>200k</td><td>8.9 / 27.6</td><td>2.5 / 7.2</td></tr>
<tr><td>10k &times; 20</td><td>400k</td><td>19.5 / 55.1</td><td>5.0 / 14.4</td></tr>
</tbody>
</table>
</div>
<figcaption>

**Table 1.** Performance of the MAYA extensions for a shuffle of *n*
ciphertexts. Timings are measured end-to-end and in seconds as MT / ST.
*B* &times; *b*: *B* blocks (e.g., districts) of *b* ciphertexts each.
*B* &times; [*b*<sub>1</sub>, *b*<sub>2</sub>, …]: *B* groups, each
partitioned into sub-blocks of sizes *b*<sub>j</sub>. For
&Pi;<sub>3</sub>, each entry describes an entity operating under an
independent key with its own block structure. For &Pi;<sub>4</sub>, we report
one mix server's shuffle-and-partial-decryption step. For &Pi;<sub>5</sub>,
*n* = 2*Bb* since each entry produces two ciphertexts (one per key).

</figcaption>
</figure>

All variants were implemented in Rust on top of the MAYA reference
implementation, using `curve25519-dalek` over Ristretto255 at 128-bit security
with folding factor k=4, on the hardware described on the
[home page]({{< relref "_index.md" >}}#performance). Timings include all
cryptographic operations, CRS regeneration, and disk I/O.

The source code for all four extensions is at
[uclcrypto/maya_extensions](https://github.com/uclcrypto/maya_extensions).

Proof sizes remain logarithmic across all variants. At n = 200,000 the measured
sizes are 6,176 bytes for the single-key variants &Pi;<sub>1</sub> and
&Pi;<sub>2</sub>; 6,272 bytes for two keys, growing by 96 bytes per additional
key up to 15,680 bytes for 100 keys (&Pi;<sub>3</sub>); 6,224 bytes for
&Pi;<sub>4</sub>; and 6,320 bytes for &Pi;<sub>5</sub>. At n = 2,000,000 the
&Pi;<sub>1</sub> proof is 7,328 bytes.
