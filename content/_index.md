---
title: "MAYA: A Short Shuffle Argument With Fast Verification"
---

**MAYA** is a shuffle argument with **O(log n)** proof size and **transparent setup**,
built for verifiable elections. It allows a mix-server to prove — succinctly and
without any trusted setup — that a shuffled list of ciphertexts is a correct
re-encryption and permutation of the original list, without revealing the
permutation itself.

<aside class="callout">
<div class="callout-title">
<span class="callout-icon" aria-hidden="true">i</span>
New to MAYA?
</div>
<div class="callout-body">
<p>Welcome! Thanks for your interest. Trying to figure out where to start?</p>
<ul>
<li>If you are a developer, head over to the <a href="{{< relref "specification.md" >}}">Verifier Specification</a> and <a href="{{< relref "getting-started.md" >}}">Getting Started</a>.</li>
<li>If you are interested in the research behind MAYA, read more about it here: <a href="https://eprint.iacr.org/2026/941.pdf">MAYA: A Short Shuffle Argument With Fast Verification</a> — <a href="https://www.sigsac.org/ccs/CCS2026/call-for/call-for-papers.html">CCS&nbsp;'26</a>, to appear.</li>
<li>If you want to see how MAYA handles the structures real elections actually have — blocks, multiple keys, decryption mixnets — read about <a href="{{< relref "mayask.md" >}}">MAYAsk</a>.</li>
</ul>
</div>
</aside>

## Performance

<figure class="data-table">
<div class="table-scroll">
<table>
<thead>
<tr><th>n</th><th class="mode">Mode</th><th class="num">Prover (s)</th><th class="num">Verifier (s)</th><th class="num">Proof size</th></tr>
</thead>
<tbody>
<tr><td rowspan="2">1,000</td><td class="mode">ST</td><td class="num">0.19</td><td class="num">0.05</td><td class="num" rowspan="2">3,872 B</td></tr>
<tr><td class="mode">MT</td><td class="num">0.09</td><td class="num">0.03</td></tr>
<tr class="alt"><td rowspan="2">10,000</td><td class="mode">ST</td><td class="num">1.84</td><td class="num">0.45</td><td class="num" rowspan="2">5,024 B</td></tr>
<tr class="alt"><td class="mode">MT</td><td class="num">0.72</td><td class="num">0.15</td></tr>
<tr><td rowspan="2">100,000</td><td class="mode">ST</td><td class="num">19.07</td><td class="num">4.72</td><td class="num" rowspan="2">6,176 B</td></tr>
<tr><td class="mode">MT</td><td class="num">7.26</td><td class="num">1.40</td></tr>
<tr class="alt"><td rowspan="2">1,000,000</td><td class="mode">ST</td><td class="num">193.72</td><td class="num">45.43</td><td class="num" rowspan="2">6,752 B</td></tr>
<tr class="alt"><td class="mode">MT</td><td class="num">71.91</td><td class="num">13.81</td></tr>
</tbody>
</table>
</div>
<figcaption>

**Table 1.** MAYA (Ristretto curve) for a shuffle of *n* ciphertexts. Timings
are measured end-to-end, including disk I/O, in seconds, for single-threaded
(ST) and multi-threaded (MT) execution. Proof size is independent of the
threading model.

</figcaption>
</figure>

All benchmarks were conducted on a laptop equipped with an Intel Core i5-1245U
processor (2 Performance-cores and 8 Efficient-cores for a total of 12 threads,
with a max turbo frequency of 4.4 GHz), 16 GB of RAM, and running Ubuntu 22.04
LTS, with `rustc` 1.88.0. MAYA uses the Ristretto255 curve
(`curve25519-dalek`) with folding factor k=4.

These figures improve on the ones reported in the paper. The gains come from
implementation-level optimizations accumulated in the codebase, not from any
change at the protocol level. See the [Paper]({{< relref "paper.md" >}}) for the full evaluation,
including the comparison against Verificatum.

## Learn more

- [Paper]({{< relref "paper.md" >}}) — the full research paper describing MAYA
- [Specification]({{< relref "specification.md" >}}) — the verifier specification
- [MAYAsk]({{< relref "mayask.md" >}}) — extensions for blocks, multiple keys, and decryption mixnets
- [Getting Started]({{< relref "getting-started.md" >}}) — run MAYA yourself
- [Code]({{< relref "code.md" >}}) — source code and repositories
