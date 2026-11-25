# ECHO / SHIFT — Requirements v1.0

Authoritative detailed design: [Phase 2 design specification](docs/specs/2026-11-24-echo-shift-design.md). Original concept v1.2 remains unchanged.

| ID | Requirement | Acceptance evidence |
|---|---|---|
| G01 | Complete Arrival, Bridge, Gate in order | End-to-end completion test |
| G02 | Controlled movement and collision | Boundary, gap, locked/unlocked exit tests |
| G03 | Low, mid, low then high | Correct and incorrect pulse sequence tests |
| G04 | Immediate transformations and feedback | State and browser rendering checks |
| G05 | Reset and replay | Chamber reset and full replay tests |
| C01 | Cinematic fixed camera, approved art language | Visual review at desktop/mobile |
| C02 | Progressive contextual guidance | First-time prompts and optional hints review |
| C03 | Adaptive atmospheric soundscape | Audio review and ducking checks |
| C04 | Seeded decorative variation | Same-seed equality and cross-seed gameplay invariance |
| A01 | Muted equivalent access | Complete all puzzles with audio disabled |
| A02 | Keyboard and touch | Separate end-to-end input checks |
| A03 | Reduced motion, focus, labels, responsive UI | Browser accessibility and viewport checks |
| P01 | Static self-contained deployment | Local HTTP smoke test, no external requests |
| P02 | Document actual AI-assisted workflow | Prompt/output/decision provenance review |
| P03 | Release artifacts and case study | ZIP inspection, source and documentation review |

Target 3–5 minutes; no forced waiting. Independent human playtesting is a separate proposed activity. All evidence is pending implementation.
