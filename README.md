# jts — JavaScript to Slint

Compact, token-efficient Slint reference for AI coding assistants.

**Purpose:** Slint (`.slint`) is the UI language. JS and Python here are only *translations* — familiar
syntax that helps an AI understand Slint concepts faster. jts is a guide for writing Slint with **any
backend**: JS/Node, JS/WASM on the web, Python, C++, or Rust. The `.slint` rules apply to all of them;
only the host-side glue (load, props, callbacks, models) differs.

**AI: do this first**
1. **Check the official Slint AI skill: [slint-ui/ai-plugins](https://github.com/slint-ui/ai-plugins)** —
   `skills/slint/SKILL.md` and `skills/slint/reference/*` (gotchas, layout, interop). If it is
   installed as a plugin/skill, use it; otherwise fetch it:
   `gh api repos/slint-ui/ai-plugins/contents/skills/slint/SKILL.md -H "Accept: application/vnd.github.raw"`
2. Then read [`INDEX.md`](INDEX.md) and load only the jts file(s) you need. jts is a small complement
   to the official skill (extra gotchas, JS/Python API notes), not a replacement.

Verified against Slint 1.x docs (docs.slint.dev). Verify version-sensitive items against the installed version.
