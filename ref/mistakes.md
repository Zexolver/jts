# Common mistakes (check before writing code)

0. **Unwanted stretching (most common layout bug)**: an element with no explicit size fills its parent, and children in a layout share leftover space, so a Rectangle/layout with no `height` swells to fill the window. Fix, pick one:
   - `height: 40px;` (or `min-height`/`max-height`; `preferred-height` only a hint, layout may still grow it)
   - `vertical-stretch: 0;` (same for `horizontal-stretch` / width) so it stays at its preferred size
   - `alignment: start;` on the parent Vertical/HorizontalLayout so children keep natural size and pack to one end
   - Nested layout that must hug its content: give the wrapping Rectangle/layout `vertical-stretch: 0` too.
   Set an explicit height on every Rectangle/container you don't want to fill; text/buttons size to content by default.

1. **Naming**: `.slint` `my-prop` -> Py `comp.my_prop`. JS: official examples and the slint-ui README use `comp.my_prop` (underscores or as-declared); the docs site says camelCase. Prefer `my_prop`; if undefined, try the others. Same for callbacks/globals' members.
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
15. **String interpolation** is `"Count: \{root.count}"` (backslash-brace), NOT `${x}` or `{x}`.
16. **Units are distinct types**: no `em` (use `rem`); float->length error: `value * 1px`, back with `len / 1px`; angles `* 1deg`. `/` is never integer division (`.floor()`).
17. **Colors**: no `hsl()`; use hex, `rgb()`, `rgba()` (alpha 0..1), `hsv()`, `oklch()`.
18. **`padding`/`spacing` only work on layouts**. To inset a Text wrap it: `HorizontalLayout { padding-left: 6px; Text {} }`.
19. **`=` vs `:`**: `prop: expr;` is a reactive binding at element scope; `prop = expr;` assigns inside callbacks. `in` props can't be assigned inside the component. Calling callbacks/functions from a binding requires them `pure`.
20. **Don't bind defaults**: an explicit `x: 0` inside a layout overrides layout placement.
21. **Fill rules**: Rectangle, TouchArea, FocusScope and every layout fill their parent; Text and Image take preferred size. Outside a layout, an implicitly sized element with no x/y is *centered* (set `x: 0; y: 0;`).
