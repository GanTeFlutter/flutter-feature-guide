# Service Rules

## Decision tree

1. **Does it exist in the locator?** -> Check `service_locator.dart`. If yes, retrieve it with `locator<Service>()`.
2. **Is it a shared/general service?** -> Add it under `lib/product/service/` + register in the locator.
3. **Is it specific to this module only?** -> Add it under `service/` in the feature folder, do not add to locator.

## Adding a shared service

Single-file service -> `lib/product/service/services/<service_name>.dart`
Large/multi-file service -> Create `lib/product/service/<service_name>/` folder

Register in locator (`service_locator.dart`):
```dart
// Inside _registerSingletons:
..registerSingleton<NewService>(NewService.instance)

// Inside extension:
NewService get newService => locator<NewService>();
```

## Module-specific service

Added under `service/` in the feature folder. **Not registered** in the locator.
Used only by that module's Cubit.

```
lib/future/<feature_name>/
  service/
    <feature>_service.dart
```
