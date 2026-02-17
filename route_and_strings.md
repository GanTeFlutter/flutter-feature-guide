# Route Addition & Strings

## Route Addition (TypedGoRoute)

The project uses type-safe `TypedGoRoute` + `GoRouteData` + code generation.
Routes are defined in `lib/product/navigation/app_router.dart`.

### Adding a new route (2 steps):

**1. Create a GoRouteData class** (`app_router.dart`):
```dart
class FeatureRoute extends GoRouteData with $FeatureRoute {
  const FeatureRoute();

  @override
  Page<void> buildPage(BuildContext context, GoRouterState state) {
    return slideRightTransition(
      key: state.pageKey,
      child: const FeatureView(),
    );
  }
}
```

**2. Add to TypedGoRoute annotation** (nested under HomeRoute):
```dart
@TypedGoRoute<HomeRoute>(
  path: '/home',
  routes: [
    // ...existing routes
    TypedGoRoute<FeatureRoute>(path: 'feature_name'),
  ],
)
```

**3. Run codegen:**
```
dart run build_runner build --delete-conflicting-outputs
```

### Passing data ($extra)

Use the `$extra` parameter to pass data to routes:
```dart
class FeatureRoute extends GoRouteData with $FeatureRoute {
  const FeatureRoute({required this.$extra});
  final FeatureConfig $extra;

  @override
  Page<void> buildPage(BuildContext context, GoRouterState state) {
    return slideRightTransition(
      key: state.pageKey,
      child: FeatureView(config: $extra),
    );
  }
}
```

### Navigation usage

```dart
// Navigate to page
const FeatureRoute().go(context);

// Push onto stack
const FeatureRoute().push(context);

// Navigate with data
FeatureRoute($extra: config).go(context);
```

### Key rules

- Sub-routes are added nested under `HomeRoute`
- Every route class must include `with $RouteName` mixin
- Use `buildPage` override for custom transitions (slide/fade)
- `route_transitions.dart` provides `slideRightTransition` and `fadeTransition`

---

## String Addition

All UI texts are defined as static constants in `lib/product/const/app_string.dart`.
Never use hardcoded strings directly in widgets.

**When adding new strings:**

1. Open `lib/product/const/app_string.dart`
2. Add the new string(s) following the existing naming convention:
   ```dart
   static const String featureTitle = 'Title';
   static const String featureDesc = 'Description';
   ```
3. Reference in widgets as `AppString.featureTitle`
