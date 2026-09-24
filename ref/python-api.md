# Python API (`pip install slint` / `uv add slint`, Python 3.12+)

```python
import slint

class App(slint.loader.app.MainWindow):      # auto-loads app.slint from sys.path
    @slint.callback                          # binds to callback `submit` (name match, snake_case)
    def submit(self, text: str):
        self.total += 1

app = App()
app.name = "Joe"
app.model = slint.ListModel([1, 2, 3]); app.model.append(4)
app.run()
```
- Explicit: `comps = slint.load_file("app.slint"); w = comps.MainWindow()`.
- Names: `my-prop`->`my_prop`. `@slint.callback(name="some-name")` to override.
- Types: int/float->int/float, string->str, bool->bool, color->`slint.Color`, image->`slint.Image`, struct->object/dict-like, `[T]`->`slint.Model`/`ListModel`.
- Globals: `w.Logic.total`, `w.Logic.compute = fn`, or `@slint.callback(global_name="Logic", callback_name="compute")` in the subclass.
- Async: `slint.run_event_loop(coro)`, `slint.Timer`; main thread only for UI; from other threads use `loop.call_soon_threadsafe`; `asyncio.to_thread` for blocking work.
