# ECHO / SHIFT — Technical architecture v1.0

Proposed greenfield static browser implementation; no existing game code assumed.

| Path | Responsibility / interface |
|---|---|
| index.html | Accessible shell, start, scene, controls, settings, completion |
| css/game.css | Responsive layout, tokens, focus, reduced motion |
| config/resonance-atrium.json | Chamber bounds, walkable polygons, resonator and exit locations, pulse sequences |
| config/design-tokens.json | Palette, typography, spacing, effects |
| js/game-engine.js | createState(config, seed), reduce(state, action, config), getAvailableActions(state, config) |
| js/environment.js | createDecoration(seed, chamberId), renderScene(state, config, decoration) |
| js/audio-engine.js | unlock(), preview(pulse), respond(event), setMix(settings), dispose() |
| js/controls.js | bindControls(dispatch), returns cleanup function |
| js/main.js | Load configuration, dispatch actions, render state, route audio events |
| tests/ | Pure state tests and browser integration tests |

State: chamber index, position, sequence index, unlocked flag, completion, seed, settings, feedback. Actions: MOVE, LISTEN, PULSE, HINT, RESET_CHAMBER, CROSS_EXIT, REPLAY, SET_SETTINGS. Reducer returns {state, events}; events describe preview, mismatch, success, transition and completion. Only reducer changes gameplay. MOVE clamps or rejects illegal paths; crossing requires unlocked exit and proximity. Incorrect pulse preserves accepted sequence prefix. LISTEN previews next required signature without advancing. Audio errors are caught at the audio boundary and never block state updates. Decorative seeded random stream is independent of gameplay; renderer never changes collision. Settings persist in memory for session; no personal data or analytics. Serve via local HTTP or static hosting; no runtime dependencies on remote assets. Browser audio requires user gesture. Tests use Node built-in test runner and browser automation if available; physical device and listening review remain separate.
