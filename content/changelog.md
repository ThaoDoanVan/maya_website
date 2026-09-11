## 1.0 — September 2026

Initial public release of MAYA.

- **Verifier Specification 1.0.** First public version of the
  [Verifier Specification]({{< relref "specification.md" >}}).
- **Hash function.** Specification 1.0 uses HMAC-SHA-256 as its only hash
    function, whereas the code evaluated in the CCS '26 paper uses a Merlin
    transcript. Proofs produced by that code therefore cannot be checked by a
    verifier that follows Specification 1.0, and vice versa.
- **Performance.** The figures on the
    [home page]({{< relref "_index.md" >}}#performance) are better than those in
    the CCS '26 paper because the implementation improved. The protocol is
    unchanged.

## CCS '26 artifact

    The code evaluated in the CCS '26 paper, which reproduces all the measurements
    reported there, is available at
    [uclcrypto/maya_shuffle_proof](https://github.com/uclcrypto/maya_shuffle_proof).
