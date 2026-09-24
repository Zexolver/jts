# Minimal working example

app.slint
```slint
import { Button } from "std-widgets.slint";
export component MainWindow inherits Window {
    in-out property <int> counter: 0;
    callback bump();
    VerticalLayout {
        Text { text: "Count: " + counter; }
        Button { text: "+1"; clicked => { root.bump(); } }
    }
}
```
main.mjs
```js
import * as slint from "slint-ui";
const ui = slint.loadFile(new URL("app.slint", import.meta.url));
const w = new ui.MainWindow();
w.bump = () => { w.counter += 1; };
await w.run();
```
main.py
```python
import slint
class App(slint.loader.app.MainWindow):
    @slint.callback
    def bump(self): self.counter += 1
App().run()
```
