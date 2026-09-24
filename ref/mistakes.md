# Common mistakes (check before writing code)

1. **Naming**: `.slint` `my-prop` -> JS `comp.myProp`, Py `comp.my_prop`. Same for callbacks/globals' members.
2. **Private props unreachable**: default visibility is `private`. Declare `in`, `out`, `in-out` (or `private`) explicitly to expose to host.
3. **`in` props are host-writable only**; `out` props are host-readable only (set inside .slint). Use `in-out` for both.
4. **Models are copied in JS**: `comp.items.push(x)` does nothing. Reassign: `comp.items = [...comp.items, x]`. In Python use `slint.ListModel` and `.append()`.
5. **Missing semicolons**: every property assignment/statement ends with `;`.
6. **Callback assignment syntax differs**: in .slint `clicked => { ... }`; in JS `comp.clicked = () => {...}`; in Py `@slint.callback` method or assignment.
7. **Component must be `export`ed** and derive from `Window`/`Dialog` (or `export component X inherits Window`) to be instantiated from host.
8. **Units required**: lengths need `px`/`phx`/`rem` (`width: 100px`), durations `ms`/`s`, angles `deg`. Bare numbers only for unitless types.
9. **Layouts**: children of `VerticalLayout`/`HorizontalLayout`/`GridLayout` ignore x/y. Outside layouts, elements need explicit x/y/width/height or they overlap.
10. **String concat with numbers** works in .slint (`"n: " + n`); JS numbers arrive as `Number`, ints get truncated.
11. **Event loop**: JS `await comp.run()` (shows + runs). Python: `comp.run()`. Create/touch UI only from the main thread.
12. **Imports** of other .slint: `import { Foo } from "foo.slint";`, path relative to the importing file.
13. **Globals** must be `export global` to reach from host; in JS `comp.MyGlobal.prop`.
14. Don't invent widgets: std widgets are `import { Button, LineEdit, ... } from "std-widgets.slint";`.
