# Common host-glue patterns (JS / Python)

Pattern: keep state and logic in the host, keep `.slint` a pure view. Push data in via
`in`/`in-out` properties and globals, pull actions out via callbacks. See ai-plugins
`interop.md` for the same pattern in Rust/C++.

## Editable list (model in, edits out)

slint: `in-out property <[{title: string, checked: bool}]> items;` in a `ListView`/`for`.

JS — mutate through `ArrayModel`, don't reassign the array on every change:
```js
const model = new slint.ArrayModel(items);
win.items = model;
win.add_item = (title) => model.push({ title, checked: false });
win.remove_item = (i) => model.remove(i, 1);
```

Python — same with `slint.ListModel`:
```python
model = slint.ListModel(items)
win.items = model
@slint.callback
def add_item(self, title): self.items.append({"title": title, "checked": False})
```

## Dialog / confirm

slint: `export component ConfirmDialog inherits Dialog { StandardButton { kind: ok; } StandardButton { kind: cancel; } }`
JS: `const d = new ui.ConfirmDialog(); await d.run();` then read a result property set by the
`StandardButton`'s handler, or check which button closed it via a callback set before `run()`.

## Push async/background data into the UI

Keep Slint calls on the main thread. Do the work off-thread, then hand the result back:
- JS: any `await` before touching `win.*` is fine since Node is single-threaded; just don't call
  UI setters from a `worker_thread` — post a message back to the main thread instead.
- Python: run blocking work with `asyncio.to_thread`, or from a real OS thread call
  `loop.call_soon_threadsafe(lambda: setattr(win, "progress", p))` — never set properties directly
  from another thread.

## Theming from the host

Prefer an in-`.slint` `Theme` global driven by `Palette.color-scheme` (ai-plugins
`icons-and-theming.md`) over pushing colors from the host — the host only needs to set an
enum/bool `in property` like `Theme.preference` if the user can override light/dark in-app.

## Timers / polling from Slint itself (no host loop needed)

slint: `Timer { interval: 1s; running: true; triggered => { root.tick += 1; } }` — often simpler
than a host-side timer when the driving state already lives in `.slint`.
