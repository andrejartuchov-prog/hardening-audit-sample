# Security & Reliability Hardening Audit — a worked sample

**Live walkthrough → https://hardening-audit.previewlab.app**

A security and reliability hardening audit, run end to end on an anonymized open-source FastAPI payments platform — the same class of system I am asked to audit. The run, the mutations and the test output are real; the platform's name, files and symbols are anonymized. No client code, systems or data are involved at any point.

## The question it answers

A green test suite tells you the tests pass. It does not tell you they check anything. If you have thousands of green tests and want to know whether they would catch a real defect, this sample answers that in concrete form: I take a green suite, break the logic underneath it on purpose, and show whether the suite notices.

Twice, it did not.

- **Money** — I disabled the limit that stops a customer spending more credit than they hold. A customer with $10 of credit could settle a $50 order. Tests: green before, green after.
- **Access** — I removed tenant isolation for one caller type. One tenant's ordinary token could read every other tenant's orders, amounts and customers. Tests: green before, green after.

Each time I measured against a baseline first, so "green after" means something: the suite was green before the change too, and stayed green through a defect that ships real money and real data to the wrong place.

## The method

Four axes: security, data & compliance, money correctness, and reliability. Every finding arrives in one shape — priority, what is broken, how to reproduce it, how to fix it — with reproductions that run rather than reproductions that are described. Where an axis comes back clean, that is a result too, and it ships with the evidence that the check actually ran. A finished audit is coverage of the agreed checks, not a tally of holes.

## What the walkthrough shows

Three findings in the shape you would receive them, and the mutation sample above in full — the broken code, the green run beside it, and why the tests stayed silent on both.

The audit runs on a scoped read-only copy: no production access, no production secrets.
