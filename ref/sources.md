# Sources / where to look deeper (all on GitHub, verified to exist)

- **slint-ui/ai-plugins** — **READ THIS FIRST.** OFFICIAL Slint skill for AI agents (Claude/Cursor). `skills/slint/reference/`: gotchas.md (compiler errors — mined into mistakes.md), language-and-layout.md
(mined into mistakes.md/slint-syntax.md), interop.md (Rust/C++ host glue — jts only covers JS/Python),
polish.md (spacing/color/widget-state checklist for making a working UI look good — not duplicated
here, read it directly), events-and-overlays.md (TouchArea/FocusScope details, context menus, popovers
— key points mined into mistakes.md #22-24), icons-and-theming.md (Theme global pattern — mined into
patterns.md/widgets.md), debugging-and-mcp.md (rendering a screenshot / MCP server for visual review —
not covered in jts, read it directly). Best upstream source; jts should stay a smaller JS/Py-focused complement.
- **slint-ui/slint** `examples/*/node/main.js` (todo, carousel, memory, system-tray, virtual_keyboard main.ts), `demos/printerdemo/node`, `demos/home-automation/node` — real JS usage. `api/node/README.md`, `api/python/` (Python API src+tests).
- **slint-ui/slint-nodejs-template**, **material-nodejs-template** — starter JS projects.
- **slint-ui/slint-python-template**, **material-python-template** — starter Python projects.
- Community Python: `wojtmic/lazurich`, `uAtomicBoolean/xkcd-reader`, `loosebird/flock-vpn`, `ilius/pyglossary` (ui_slint). `g9wp/slint-gen-tsd` generates .d.ts from .slint.
- **slint-ui/madewithslint** — list of apps built with Slint.
Fetch raw file: `gh api repos/OWNER/REPO/contents/PATH -H "Accept: application/vnd.github.raw"`.
