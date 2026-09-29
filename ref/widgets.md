# std-widgets.slint cheat-sheet

`import { Button, ... } from "std-widgets.slint";` — prefer these over hand-rolled controls:
focus, hover, keyboard and accessibility come free and match the OS style (Qt/Fluent/Cupertino/
Material/Cosmic per platform).

**Controls**: `Button` (`text`, `icon`, `primary`, `enabled`, `clicked`), `CheckBox` (`checked`,
`text`, `toggled`), `ComboBox` (`model: [string]`, `current-index`, `current-value`, `selected`),
`RadioGroup` (`RadioButton` children or `model`, `checked-value`), `Slider` (`minimum`, `maximum`,
`value`, `changed`), `SpinBox` (`minimum`, `maximum`, `value`, `edited`), `Switch` (`checked`,
`text`, `toggled`), `ProgressIndicator`/`Spinner` (`progress` 0..1, `indeterminate`),
`StandardButton` (`kind: ok/cancel/yes/no/...`, `clicked`).

**Text/lists**: `LineEdit` (`text`, `placeholder-text`, `input-type: password`, `accepted`,
`edited`), `TextEdit` (multi-line, `wrap`), `ListView`/`StandardListView` (`model`, needs explicit
height or a filling parent — see mistakes.md #0), `StandardTableView` (`columns: [{title}]`,
`rows: [[{text}]]`).

**Layout helpers**: `VerticalBox`/`HorizontalBox` (= layout + sane default padding/spacing — prefer
over raw `VerticalLayout`/`HorizontalLayout` with hand-set padding), `GridBox`, `GroupBox` (`title`).

**Other**: `TabWidget` (`Tab { title }` children), `DatePickerPopup`/`TimePickerPopup`,
`AboutSlint`, `Palette` (theme colors: `background`, `foreground`, `alternate-background`,
`control-background`, `accent-background`, `border`, `color-scheme` — read-only unless you set
`color-scheme` to force a theme; see ai-plugins `icons-and-theming.md` for a full `Theme` global
pattern), `MenuBar`/`ContextMenuArea`/`PopupWindow` for menus/popups (see ai-plugins
`events-and-overlays.md`).

Full property/callback list per widget: docs.slint.dev → Reference → std-widgets. When unsure a
widget exists, check this list before inventing one (mistakes.md #14).
