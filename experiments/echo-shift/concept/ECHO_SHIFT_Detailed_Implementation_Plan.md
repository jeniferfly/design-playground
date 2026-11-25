# ECHO / SHIFT Phase 2 Implementation Plan

> For agentic workers: execute task by task, using checkboxes for tracking. Stop at each approval gate. No implementation authorized by this document alone.

**Goal:** Deliver a polished, original 3–5 minute playable Resonance Atrium and coordinated portfolio release.

**Architecture:** Retain static HTML/CSS/JavaScript, SVG 2.5D, Web Audio, JSON configuration and a deterministic reducer. Reuse the shipped v0.2 where useful, but isolate new development and make the tested engine the runtime engine.

**Tech stack:** Browser-native APIs, Node built-in test runner, browser automation where available, manual browser/device/audio review.

## Global constraints

Preserve concept v1.2, planning v1.0 and all G01–G05, C01–C04, A01–A03, P01–P03 requirements. Current user refinements add gates and portfolio polish; no combat, live AI, backend, microphone, accounts or commercial-scale systems. Preserve shipped v0.2 byte-for-byte. Preserve portfolio navigation: external/PDF/game/GitHub links open new tabs with noopener noreferrer and accessible descriptions, no visible '(new tab)' text; Back to Portfolio stays in current tab; download PDF remains available. No publishing or commits without explicit authorization. All paths below are proposed relative to a separate enhancement workspace unless identified as observed.

## Baseline audit and pre-development gate (G0)

Observed in v0.2 ZIP: index.html, play.html, style.css, js/engine.mjs, js/environment.mjs, js/audio.mjs, js/main.mjs, js/play.js, tests/engine.test.mjs, assets/atrium.svg, docs/Validation.md. The shipped bundle duplicates engine logic; approach and exit shortcuts reposition or advance directly. Existing tests do not establish browser behavior. Prior browser automation blocked; no independent playtests. Preserve originals. Proposed review numbering v0.3 (Arrival), v0.4 (Bridge), v0.5 (Gate), v0.6 (portfolio polish), v1.0 (approved release). Obtain approval of this plan before visual exploration; obtain Golden Standard approval before production code.

## Task 1 — Golden Standard (G1; no gameplay implementation)

- [ ] Produce original concept explorations using fixed brief derived from PDF pages 2, 5, 6. Compare 2–3 meaningful treatments for depth, silhouette and cue readability; record actual prompts, tool, outputs, selected/rejected rationale in docs/AI_Creative_Workflow.md and docs/Creative_Decision_Log.md.
- [ ] Deliver assets/references/arrival-locked.png and arrival-open.png (representative full scene with UI), mobile composition, and docs/Visual_Golden_Standard.md with annotated camera, palette, type, spacing, character scale, material, glyphs, locked/preview/mismatch/success and reduced-motion treatments. Confirm actual colors against original artwork; do not assume provisional swatches are exact.
- [ ] Review side by side against concept v1.2 p5: same monumental slate/stone architecture, mint waves, amber traveler, quiet atmosphere; readable path and character; no decorative occlusion. Review at desktop and mobile size; check UI contrast and target sizes (44 CSS px where practical). Distinguish static design reference from runtime capture. Approve before any production implementation. Lock approved references/tokens; changes require targeted review.

## Task 2 — Chamber A vertical slice (G2)

Files: proposed config/resonance-atrium.json, config/design-tokens.json, js/game-engine.mjs, js/environment.mjs, js/audio-engine.mjs, js/controls.mjs, js/main.mjs, play.html, css/game.css, tests/arrival.test.mjs, tests/browser/arrival.spec.mjs. Existing index.html retained as portfolio shell. Use native modules via HTTP; if direct-file launch must remain supported, produce a generated local bundle from the same tested modules, never manually duplicate logic.

Interfaces: createState(config, seed), reduce(state, action, config) -> {state, events}, getAvailableActions(state, config); createDecoration(seed, chamberId), renderScene(state, config, decoration); unlock(), preview(pulse), respond(event), setMix(settings), dispose(); bindControls(dispatch) -> cleanup. Actions MOVE, LISTEN, PULSE, HINT, RESET_CHAMBER, CROSS_EXIT, REPLAY, SET_SETTINGS. State and events as in approved architecture. Only reducer mutates game state; audio/render consume it. Configuration owns collision polygons and interaction locations.

- [ ] Write failing tests for movement and collision: legal displacement changes position, wall/locked passage and oversized step cannot cross forbidden area; alternate movement follows legal path, not teleport. Run `node --test tests/arrival.test.mjs`; record assertion failure, then implement bounded substeps or swept collision, screen-relative controls and legal waypoint navigation. Repeat same command; expect pass.
- [ ] Add failing tests: distant LISTEN/PULSE cannot progress; nearby LISTEN emits low preview without progress; mid/high mismatch preserves state; low opens passage; exit only reachable and crossable when open and near; reset restores safe spawn and lock while retaining settings/seed. Implement minimal reducer behavior and rerun. At A-only slice, crossing ends slice review, not full game completion.
- [ ] Add focused tests for deterministic decoration (same seed same result; different seed changes decoration only), settings retention and audio event mapping. Implement renderer matching approved locked/open reference, distinct numbered wave glyphs, immediate state-derived feedback, visible interaction radius, polite live region, hints, touch pad and keyboard controls. Audio unlock only on gesture; original low/mid/high timbres, conservative mix, ambience ducking, success/mismatch; failed audio initialization must not block play. Reduced motion suppresses decorative motion/transitions.
- [ ] Browser checks: serve `python -m http.server 8000` from workspace; open /play.html. Complete A with keyboard, touch and muted; check locked route, legal alternate navigation, reset, focus, resize, no clipping, no console errors, no remote requests. Capture locked/open screenshots against Golden Standard; listen to actual audio, check volume/mute/ambience and no stuck tones. If automation unavailable record manual evidence and limitation; do not claim unobserved results. Run `node --test tests/*.test.mjs`. Gate G2: user approves playable A and visual parity; stop if any blocking defect.

## Task 3 — Extend approved system to Bridge and Gate (G3)

Files: config/resonance-atrium.json, js/game-engine.mjs, js/environment.mjs, js/audio-engine.mjs, tests/bridge.test.mjs, tests/gate.test.mjs. Consume same Task 2 interfaces; no separate room-specific engine.

- [ ] Bridge red tests: mid only unlocks; gap cannot be crossed before reveal, including large movement and alternate path; after reveal only bridge polygon permits crossing; reset restores gap; exit requires legal proximity. Run `node --test tests/bridge.test.mjs`; record expected failure. Implement config and state-derived bridge rendering/collision; rerun to pass. Review Bridge art and reduced guidance against Golden Standard before Gate.
- [ ] Gate red tests: high first does not advance; low advances one visible step; mismatch preserves accepted prefix; high then opens; reset clears both; exit after alignment completes; repeated completion cannot double-trigger. Run `node --test tests/gate.test.mjs`; record failure, implement and rerun. Both sequence steps visible throughout; listening previews next required signature.
- [ ] Verify A→B→C progression, safe spawn, fixed cameras, reduced-motion cuts, adaptive harmonic layers, same-seed reproducibility and identical collision/solutions across seeds. Run `node --test tests/*.test.mjs`; browser complete all chambers with keyboard and touch, repeat muted. Review actual screenshots and listening results. Gate G3: approve complete level, including each transformation and visual continuity.

## Task 4 — Portfolio journey and release (G4, G5)

Files: index.html, play.html, css/game.css, js/main.mjs, tests/journey.test.mjs, docs/Validation_Report.md, docs/AI_Creative_Workflow.md, docs/Creative_Decision_Log.md, README.md, CHANGELOG.md, case-study/ECHO_SHIFT_Case_Study_v2.0.pdf.

- [ ] Red tests for start/onboarding, replay and settings: gameplay input inactive before Begin; no audio before gesture; completion only after C exit; replay resets all rooms and preserves seed/settings; new variation if approved changes seed only; reset current chamber does not restart entire journey. Run `node --test tests/journey.test.mjs`; implement minimal state/screen transitions; rerun. Start screen introduces movement, listening, pulses and accessibility options; onboarding teaches through A. Completion reflects actual accomplishment, offers replay and return navigation. No forced waiting or punitive scoring.
- [ ] Review responsive portfolio page and game screens; preserve v0.2 link behavior, existing repository destination until a verified project URL is supplied. Test PDF view/download, new tabs and same-tab return, keyboard focus and no visible new-tab suffix. G4 user approves full visitor journey.
- [ ] Run full Node suite, syntax checks, browser smoke and end-to-end tests on supported browsers, desktop/mobile viewport, physical touch and listening review when available. Test focus, contrast, labels, live announcements, reduced motion, audio failure, no external requests. Record actual elapsed completion times (target 3–5 minutes, not enforced). Proposed independent study: 5–8 consenting players, uncoached and muted repeat; record confusion, retries, time to first success, completion and enjoyment; no fabricated data. If unavailable explicitly mark unperformed and seek release decision on residual limitations.
- [ ] Create case study only from actual screenshots, documented prompts/outputs/decisions, implementation and verified evidence. Include limitations and distinguish AI generation, AI-assisted coding and seeded procedural generation. Package static site, source, tests, docs and original PDF plus new case study; verify ZIP integrity and links. Run `node --test tests/*.test.mjs` again on extracted source. G5 user approves release; publish only on separate authorization.

## Continuous documentation

For each meaningful exploration or technical iteration record date, stage, tool/model if known, actual prompt or concise faithful excerpt, input/output reference, alternatives, evaluation criteria, selection/rejection, human approval, change and validation evidence. Do not reconstruct missing prompts as historical fact. Label generated imagery as generative AI, coding assistance as AI-assisted development, and seeded algorithmic decoration as procedural (not AI). Record original/licensed asset provenance. Keep documentation alongside work; no private credentials or personal data.

## Unresolved decisions and recommendations

- Golden Standard exact typography, palette values, character proportions, camera and effect treatments: decide at G1 from actual visual studies.
- Direct file launch versus HTTP-only: recommend preserving v0.2 direct-file convenience with generated bundle if practical; confirm before Task 2. Static hosted deployment required either way.
- New variation button: recommend retain existing v0.2 option on start/completion only, preserving replay seed; confirm at G0.
- Browser/device coverage and independent playtester availability: confirm before G5; no claim of testing unavailable devices.
- GitHub project-specific destination: existing verified repository remains until exact project URL supplied.
- No commits planned without explicit permission. No implementation, visual approval or tests performed by this planning revision.
