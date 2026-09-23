# Verification

Applies to every agent. A claim is only as good as the check behind it.

- **Run it, don't read it.** If an executable source of truth exists (parser, test, running service, real engine), execute it instead of inferring from docs or code reading.
- **Verify against an independent source.** A check derived from the same assumptions as the thing it checks proves nothing. Compare generated output with something not derived from the generator.
- **Use a control.** When every case of a test fails (or passes) identically, suspect the harness before the hypothesis. Mutate one variable inside a known-good case.
- **Test the exact artifact.** Test the full line or query the artifact actually contains, not a hand-built shape that includes the token.
- **Enumerate the class.** When one defect is confirmed, scan for the whole class instead of counting only the instance under discussion.
- **Re-derive, don't relay.** A number repeated by several agents has still only been checked once. Re-check before building on a peer's figure.
- **Check history before explaining causes.** Use `git log` / `git show --stat` to find how something got there, instead of reconstructing a story from the artifact.
- **Rank findings.** Say which findings matter. An unranked list suggests they all weigh the same.
- **Silence is not success.** A run that reports success while items produced nothing is a failure. Compare requested vs. completed counts.
