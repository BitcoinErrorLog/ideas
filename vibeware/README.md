# Vibeware

Sensitive software: software whose cheap mutability is disciplined by real use, qualification, selection, and explicit boundaries.

The canonical loop:

```
use → qualify → generate → expose → select → use
```

Most captured signals should die at qualification. Most generated candidates should never become permanent. The assurance burden follows the consequence of failure, not whether a human or an agent wrote the code. Users are a sensor, not an oracle.

The idea generalizes from SHAcollision's personal experiment: a vibe-coded health app connected to his fitness ring and watches, with aggressive telemetry about how he used the software itself. Features he used more exposed more functionality. Features he ignored disappeared. New ideas were inserted to see whether they survived contact with his real behavior.

Bitdefender used "vibeware" in 2026 for AI-generated malware. This work uses the word independently and in a different sense.

- [Vibeware: The Age of Sensitive Software](vibeware.md)
- [The Vibeware Playbook](vibeware-playbook.md) (implementation spec: schemas, predicates, halt gates)
- [Prior-art claim chart](prior-art-claim-chart.md)
