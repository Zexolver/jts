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
22. **Right-click menus**: use builtin `ContextMenuArea`, not a hand-rolled overlay — it gets keyboard-menu-key support and accessibility for free. `MenuBar` goes directly on the `Window`.
23. **Overlay/popover panel placed directly under `Window` fills the window** by default (same rule as #0/#21) — set `height: layout.preferred-height;` to size it to content.
24. **`key-pressed` on a FocusScope only sees keys the focused child rejected.** For a shortcut that must win over a TextInput (e.g. Ctrl+A), use `capture-key-pressed`, which runs before the child.
25. **Don't hand-draw icon glyphs as inline `Path`.** Use `Image { source: @image-url("x.svg"); colorize: Palette.foreground; }` for monochrome icons that should match the theme; `Path` is for computed/animated shapes.
26. **`StyledText` / `@markdown` draws only paragraphs, lists, bold, italic, strikethrough, inline code and links.** Any other markdown (blockquote `>`, `#` heading, code fence, `---` rule, image, raw HTML) makes `StyledText::from_markdown` return `Err`, so an app that falls back to plain text shows the *whole* message as raw markdown. Rewrite those constructs first (quote -> a bar character, heading -> `**bold**`, code lines -> inline code, image -> its alt text, escape `<`), then parse; keep a plain-text fallback.
27. **`TextInput.preedit-text` is the keyboard's unfinished word** (phones keep the last word "composing" until a space). `text` does not include it. So: placeholder only when `text == "" && preedit-text == ""`, enable Send when either is non-empty, and before reading `text` to send call `input.clear-focus()` (focus-out commits the preedit), then `input.focus()`. The std `LineEdit` already does the placeholder part.
28. **Auto-growing multi-line input**: `TextInput { wrap: word-wrap; single-line: false; }` inside a `Flickable` (`content-height: max(input.preferred-height, self.height)`), box `height: clamp(input.preferred-height + padding, min, max)`; centre a one-line box with `y: (parent.height - self.height) / 2` on the Flickable (not a fixed top padding). Keep the caret visible in `cursor-position-changed(pos)` by moving `content-y`. Do not put `alignment: end` on the HorizontalLayout that holds the box: it stops the box stretching and it collapses to zero width.
29. **Renamed properties warn at build**: `Flickable.viewport-width/height/x/y` -> `content-width/height/x/y`; `rotation-angle` -> `transform-rotation`.
30. **Ids declared inside an `if` / `for` subtree are not visible outside it** (normal scoping). From outside, bind through a root property (`text <=> root.draft`); things that use an id (a Copy button calling `field.select-all()`) must sit in the same conditional subtree.
31. **After a `PopupWindow` was opened and closed, the Android Back key no longer reached our `FocusScope`** (observed; focus apparently ends up nowhere). For a phone Back button do not rely on focus: expose a `public function go-back() -> bool` on the root component and let the host call it first (return false to let the platform close the app).
32. **Hide an empty list row** by wrapping it: `for m in model: VerticalLayout { if m.text != "": Row {} }`. The wrapper layout with no visible child has zero height (this is the form that worked; not checked whether `for ...: if` can be written directly).
33. **`for x[i] in model` gives the index**; assign `root.current = i` in the row's `TouchArea` (works with a two-way bound `current-index`).
34. **Std widgets cannot be restyled** (corner radius, pill shape). To offer an alternative look, wrap: `component AppButton inherits Rectangle { in property <string> text; callback clicked; preferred-width: max(72px, measure.preferred-width + 32px); measure := Text { text: root.text; visible: false; } if !alt: Button { width: parent.width; height: parent.height; text: root.text; clicked => { root.clicked(); } } if alt: Rectangle { /* pill + TouchArea */ } }`. The hidden `Text` gives the wrapper a real preferred width inside layouts.
35. **Platform checks**: `Platform.os != OperatingSystemType.android` (e.g. Enter sends on desktop but is a newline on a phone, in `TextInput.key-pressed`).
