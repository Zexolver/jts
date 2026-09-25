# Node.js / JavaScript API (`npm install slint-ui`)

```js
import * as slint from "slint-ui";
const ui = slint.loadFile(new URL("main.slint", import.meta.url)); // ESM
// CJS: slint.loadFile(path.join(__dirname, "main.slint"))
const win = new ui.MainWindow({ counter: 42 });   // initial in-props optional
win.name = "Joe";                 // set in / in-out prop
console.log(win.total);           // read out / in-out prop
win.submit = (s) => console.log(s);   // implement callback
win.submit("x");                  // invoke callback
await win.run();                  // show + run loop until window closed
```

- Only `export`ed components are on `ui`. Names: package README says dashed names may be used as declared or with underscores (`my_prop`); official examples (todo) use underscores. Docs site says camelCase; unverified.
- Types: int/float->Number, bool->Boolean, string->String, color/brush->RgbaColor object, image->ImageData, struct->plain object, enum->string (or `ui.EnumName.value`), `[T]`->array.
- **Models**: assigning array copies. Update by reassigning. For live mutation use `new slint.ArrayModel([1,2])` (`.push()`, `.remove(i,n)`, `.set(i,v)` notify UI); custom models subclass `slint.Model` (`rowCount()`, `rowData(i)`, `setRowData(i,v)`, `notify.rowAdded(i,n)`...).
- **Structs**: `win.person = { name: "Ann", age: 3 }`; read returns a copy (mutating it does not update UI).
- **Globals**: `win.Logic.total = 1; win.Logic.compute = (n) => n * 2;`
- **Public functions**: `function` marked `public` in .slint callable as `win.fn(args)`.
- Window control: `win.show()`, `win.hide()`, `win.window.…` (Window handle: `.visible`, `.fullscreen`, `.size`, ...).
- Loop: `await win.run()`, or `slint.runEventLoop()` / `slint.quitEventLoop()`; `slint.runEventLoop()` returns a promise. Non-UI async work: normal `await` works alongside the loop.
- Timers: `slint.Timer`; simple: `setTimeout`/`setInterval` work but run only while the loop is running.
- Inline source: `slint.loadSource(src, "virtual.slint")`.
- Callback return values: return correct type (`() => "text"` for `-> string`).
- Official JS todo example: `app.todo_model = new slint.ArrayModel([...])`; `model.push({title, checked:false})`; `model.rowData(i)`; `model.remove(i, 1)`; `app.todo_added = (text) => {...}`; `app.run()`.
