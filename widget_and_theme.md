# Widget and Theme Rules

## Decision tree

1. **Check `product/widget/`.** Is there an existing shared widget used across the app?
   (Example: `AppCustomElevatedButton` -> standard button for all screens)
2. **If it exists, use it.** Do not create a new widget.
3. **If it doesn't exist and will be used across the app** -> Add it under `lib/product/widget/`.
4. **If it's a widget specific only to this feature** -> Add it under `lib/future/<feature>/widget/`.

## Theme / Style

1. **Check `product/theme/parts/`.** Button theme, card theme, etc. are already defined app-wide.
   (Example: `button_theme.dart` -> Global styles for ElevatedButton, FilledButton)
2. **If the global theme is sufficient** -> Do not add extra styles, the theme is applied automatically.
3. **If a specific/different design is needed** -> Override the style in the feature's `widget/` folder.

## Existing shared widgets

| Widget | Location | Usage |
|---|---|---|
| `AppCustomElevatedButton` | `product/widget/` | Standard button (text or child, haptic feedback) |

## General widget rules

- Every repeated or complex UI piece is extracted into a separate file under `widget/`
- Widgets use `final` parameters with `const` constructors
- Callbacks are received via `VoidCallback` or `ValueChanged<T>`
