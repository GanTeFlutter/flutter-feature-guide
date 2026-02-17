# View Rules

## A) StatelessWidget (default preference)

For screens where state is listened to via BlocBuilder and no lifecycle management is needed.

```dart
class <Feature>View extends StatelessWidget {
  const <Feature>View({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(...);
  }
}
```

## B) StatefulWidget + ViewModel

For screens that require lifecycle management (initState, dispose), AnimationController,
TextEditingController, ScrollController, etc.

**2 files are created:**

`<feature>_view.dart`:
```dart
import '<feature>_view_model.dart';

class <Feature>View extends StatefulWidget {
  const <Feature>View({super.key});

  @override
  State<<Feature>View> createState() => _<Feature>ViewState();
}

class _<Feature>ViewState extends <Feature>ViewModel {
  @override
  Widget build(BuildContext context) {
    return Scaffold(...);
  }
}
```

`<feature>_view_model.dart`:
```dart
import '<feature>_view.dart';

abstract class <Feature>ViewModel extends State<<Feature>View> {
  // Controllers, lifecycle methods, helper functions go here
}
```

## When to use which?

| Scenario | Preference |
|---|---|
| Only Bloc/Cubit state is sufficient | StatelessWidget |
| AnimationController, TickerProvider | StatefulWidget + ViewModel |
| TextEditingController, FocusNode | StatefulWidget + ViewModel |
| Operations requiring initState/dispose | StatefulWidget + ViewModel |

## Common view rules (for both types)

- **Responsive**: Check `isWide` with `MediaQuery.sizeOf(context).width > 600`
- **Padding**: Proportional with `screenWidth * 0.04`, clamped with `.clamp(12, 24)`
- **Spacing**: Use `Column(spacing: 16, ...)` parameter (not SizedBox)
- **Strings**: All text is defined in the `AppString` class
- **Theme**: Use `Theme.of(context).colorScheme` and `textTheme`, no hardcoded colors
- **Navigation**: Type-safe routes with `const FeatureRoute().go(context)` (see [route_and_strings.md](route_and_strings.md))
