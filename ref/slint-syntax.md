# .slint syntax cheat-sheet

```slint
import { Button, LineEdit, CheckBox, ComboBox, Slider, ListView, ScrollView } from "std-widgets.slint";

export struct Person { name: string, age: int }
export enum Mode { light, dark }

export global Logic {
    in-out property <int> total;
    callback compute(int) -> int;
    pure callback fmt(string) -> string;   // pure = no side effects
}

component Card inherits Rectangle {
    in property <string> title;            // parent -> me
    out property <bool> pressed: ta.pressed;   // me -> parent
    in-out property <int> count: 0;
    property <color> tint: #3a86ff;        // private
    callback activated(string);
    min-width: 100px;
    background: root.pressed ? tint.darker(0.2) : tint;
    border-radius: 6px;
    ta := TouchArea { clicked => { root.activated(root.title); count += 1; } }
    Text { text: title + ": " + count; }
}

export component MainWindow inherits Window {
    title: "App";
    preferred-width: 400px; preferred-height: 300px;
    in-out property <[Person]> people;     // array/model
    in-out property <string> name <=> le.text;   // two-way binding
    callback submit(string);

    VerticalLayout {
        padding: 10px; spacing: 8px; alignment: start;
        le := LineEdit { placeholder-text: "name"; }
        Button { text: "Go"; clicked => { root.submit(le.text); } }
        if name != "" : Text { text: "Hi " + name; }
        for p[i] in people : Card { title: p.name; activated(t) => { debug(t); } }
        HorizontalLayout { Rectangle { background: red; } Rectangle { background: blue; } }
    }
}
```

Facts:
- Types: `int float string bool color brush length duration angle image percent physical-length relative-font-size [T]`(array) `struct enum`.
- Units: `px phx rem pt ms s deg turn %`.
- Reserved names: `root` (component root), `self`, `parent`.
- Element ids: `name := Element { }`.
- Conditional value: `cond ? a : b`. Element conditional: `if cond : Elem {}`. Loop: `for item[index] in model : Elem {}` (also `for i in 5`).
- `animate width { duration: 200ms; easing: ease-in-out; }`
- States: `states [ active when touch.has-hover : { bg.color: blue; in { animate bg.color { duration: 100ms; } } } ]`
- Functions: `function add(a: int, b: int) -> int { return a + b; }` (`public function` to call from host).
- Callback with return: `callback f(int) -> string;`, handler `f(x) => { return "a"; }`.
- Two-way: `a <=> b`. Property change hook: `changed prop => { ... }`.
- Layout props: `spacing padding padding-left.. alignment(start|end|center|stretch|space-between|space-around) horizontal-stretch vertical-stretch min-/max-/preferred-width/height`. GridLayout uses `Row { }`.
- Colors: `#rrggbb`, `#rrggbbaa`, `red`, `Colors.blue`, `rgb(…)`, `.brighter(f) .darker(f) .with-alpha(f)`. Gradients: `@linear-gradient(90deg, red, blue)`.
- Images: `@image-url("path.png")`. Translations: `@tr("text")`.
- Math: `Math.max/min/abs/round/floor/ceil/sqrt/sin/cos/pow`. String: `.to-uppercase() .is-empty`. `debug(x)` prints.
- Comments: `//` and `/* */`.
- Inheritance: `component A inherits B {}`; `@children` places child elements passed by parent.
- Access other elements: `elem.prop`; `Palette`, `Colors` builtin namespaces.
- Sizing: no explicit size + not in a layout => fills parent. In a layout, stretch factors share free space; use `height`, `vertical-stretch: 0`, or layout `alignment: start` to stop growth (see mistakes.md #0).
