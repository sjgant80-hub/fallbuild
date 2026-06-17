# FallBuild

The sovereign Oracle APEX wedge — a drag-drop low-code app builder in one HTML file.

Part of the **TemuOracle suite** · prime 547 · MIT · ◊·κ=1

---

## For users

FallBuild lets you build small data-driven apps by dragging widgets onto a canvas. When you are done you export a single HTML file that runs anywhere — `file://`, a thumb drive, a static host. No server. No login. No telemetry.

### Quick start

1. Open `index.html` in any modern browser.
2. Drag widgets from the left palette onto the canvas — Text, Form, Table, Chart, Button, Image, Container, Filter.
3. Click a widget to edit its props in the right panel.
4. Add pages from the page tabs above the canvas. Wire buttons to navigate between them.
5. Declare a data source (demo data, inline JSON, or FallBase) and bind your Form / Table / Chart widgets to it.
6. Hit **preview** to try it live. Hit **edit** to keep building.
7. **File → Export standalone HTML** writes a single file that runs independently.

### Templates

Press **Ctrl+K** (the Ω autopilot) or click **autopilot** in the header. Type a phrase like *crud*, *dashboard*, *lead capture*, *pricing*, *internal tool* — or describe the app you want and a T3 LLM (if a key is set) will draft the JSON for you.

### Export workflow

The exported HTML carries:

- a tiny widget runtime (~10KB) that renders pages and handles actions
- the embedded app JSON (your pages, widgets, props)
- the data bindings — inline JSON is baked in directly; FallBase sources get postMessage glue so the exported app can request rows from a live FallBase instance
- its own sovereign KONOMI shim (inherits prime 547 plus any app-specific prime)

That file opens in any browser, no FallBuild needed. Give it to a colleague. Drop it on a static host. It just works.

### FallBase integration

If your app's data source is a FallBase table, the runtime listens for FallBase via the `fall-signal` BroadcastChannel and falls back to `postMessage` against the FallBase opener. Forms write rows back. Tables refresh.

### Save / load

Apps auto-save to your browser's IndexedDB. **File → Open** lists every app you've ever built here. **File → Export JSON** / **Import JSON** moves an app between machines.

---

## For developers

### Architecture

- **Single HTML file.** All CSS and JS inline. No build step. No external dependencies.
- **App model:** `{name, pages: [{name, widgets: []}], dataSources: []}` — pure JSON, fully serializable.
- **Designer:** vanilla drag-drop. The palette drags new widgets; the canvas drags existing widgets with corner-handle resize.
- **Runtime:** a separate ~10KB JS string defined inline (`RUNTIME_SRC`) that is injected into the export template along with the app JSON.
- **Persistence:** IndexedDB for the app library, primary; JSON import/export is the universal transport.
- **Cascade tiers:** T0 templates always work offline; T2 (Ollama on localhost) and T3 (BYO Anthropic/OpenAI/Gemini/OpenRouter key) are optional enhancements for natural-language app generation.

### Estate snippets

The shell includes the required estate snippets verbatim:

- Cascade T0/T2/T3
- KONOMI sovereign shim (prime 547)
- `fall-signal` BroadcastChannel (announces presence, replies to pings, broadcasts `app-build` events)
- postMessage API — supports the standard `ping` plus FallBuild's own `{action:'open-app', appJson}` to remotely load an app into the designer
- PWA manifest baked as a data: URL

### Widget runtime

`RUNTIME_SRC` is a self-contained string defining the engine that powers exported apps. It:

- reads the embedded `__FALLBUILD_APP__` global
- renders the current page's widgets
- routes button actions (`navigate page`, `submit form`, `delete row`, custom JS)
- handles FallBase postMessage glue (request rows, write rows)
- carries its own KONOMI shim inheriting prime 547

### TemuOracle suite

FallBuild is one of the sovereign wedges replacing legacy enterprise stacks:

- **FallBuild** (547) — Oracle APEX wedge · this tool
- (other wedges per the FallStudio plan)

Every wedge: one HTML file · file:// sovereign · MIT · estate dark palette · prime-stamped.

### Licence

MIT. ◊·κ=1 is the only seal.
