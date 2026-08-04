# Domain docs

Single-context layout:

```
CONTEXT.md          — glossary of domain terms (implementation-free)
docs/adr/           — architecture decision records (hard-to-reverse, surprising, trade-off decisions)
```

Consumer rules:

- Read `CONTEXT.md` before working here for vocabulary; keep it a glossary only — no specs, no implementation detail.
- ADRs live in `docs/adr/NNNN-slug.md` in the ADR-FORMAT used across the user's repos (Status / Context / Decision / Consequences). Create them sparingly: only when the decision is hard to reverse, surprising without context, and the result of a real trade-off.
- Update `CONTEXT.md` inline when a term is resolved; record ADRs when a decision crystallises.
