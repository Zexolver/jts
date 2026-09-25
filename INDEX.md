# jts index (read this, then load only what you need)

**FIRST: consult the official Slint AI skill, https://github.com/slint-ui/ai-plugins (`skills/slint/`), before or alongside jts.**
Scope: `.slint` rules are backend-independent. JS/Python are translations/examples; the same applies to
Node, WASM/web, Python, C++, Rust (see interop.md in ai-plugins for Rust/C++).

| Need | File |
|---|---|
| Common failures (unwanted stretching!) — skim ALWAYS | ref/mistakes.md |
| .slint syntax: props, callbacks, layout, for/if, states, animation, globals | ref/slint-syntax.md |
| Node/JS API: load, props, callbacks, models, globals, event loop | ref/js-api.md |
| Python API | ref/python-api.md |
| Upstream repos/examples to consult | ref/sources.md |
| Minimal working app (slint + js + py) | ref/example.md |

Rules of thumb: dashes in .slint names -> camelCase in JS, snake_case in Python.
Only `in`/`out`/`in-out` properties and `callback`s are visible from host code. Semicolons required in .slint.
