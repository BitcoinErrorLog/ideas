# Exit-Ordered Role Allocation

Exit-ordered role allocation commits a bounded sequence of roles before assigning those roles to individual terminal controllers.

The core transition is:

`C[n] --allocate(d)→ I[n](d) + C[n-1]`

The selected member receives the next role. Every survivor remains without exclusive assignment to the roles that remain.

## Documents

- [Technical paper](paper.md)
- [The Keys That Do Not Exist Yet](the-keys-that-do-not-exist-yet.md)
- [Exit-Ordered Role Allocation for Recovery and Continuity](recovery-and-continuity.md)
