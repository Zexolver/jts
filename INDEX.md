# jts index (read this, then load only what you need)

| Need | File |
|---|---|
| Common failures (unwanted stretching!) — skim ALWAYS | ref/mistakes.md |
| .slint syntax: props, callbacks, layout, for/if, states, animation, globals | ref/slint-syntax.md |
| Node/JS API: load, props, callbacks, models, globals, event loop | ref/js-api.md |
| Python API | ref/python-api.md |
| Minimal working app (slint + js + py) | ref/example.md |

Rules of thumb: dashes in .slint names -> camelCase in JS, snake_case in Python.
Only `in`/`out`/`in-out` properties and `callback`s are visible from host code. Semicolons required in .slint.
