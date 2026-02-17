# Data Storage

## A) Simple data — SharedPreferences

For storing simple settings, strings, booleans, or small values, use SharedPreferences via the mixin pattern.

Refer to **`lib/product/service/shared_preferences/MIXIN_GUIDE.md`** for full instructions.

The approach uses mixins on `SharedPreferencesService` so each concern is separated:

```
lib/product/service/shared_preferences/
  shared_preferences_service.dart    # Main service class
  MIXIN_GUIDE.md                     # Mixin pattern guide
  mixins/
    <feature>_preferences.dart       # Mixin per feature/concern
```

Example mixin:
```dart
mixin SoundPreferences on SharedPreferencesServiceBase {
  static const String _soundKey = 'sound_enabled';

  bool get isSoundEnabled => prefs.getBool(_soundKey) ?? true;
  Future<void> setSoundEnabled(bool value) => prefs.setBool(_soundKey, value);
}
```

The service is already registered in the locator and initialized at app startup. Access it via `locator.spService`.

## B) Large data / Caching — Hive CE

For storing large data sets, lists of models, or cached content, use Hive CE.

**Packages:**
```yaml
# dependencies:
hive_ce: ^2.11.3

# dev_dependencies:
hive_ce_generator: ^1.10.0
```

Each cache concern gets its **own subfolder** under `lib/product/cache/`:

```
lib/product/cache/
  <cache_name>/
    <cache_name>_cache.dart          # Box open/close, CRUD operations
    <cache_name>_adapter.dart        # (if needed) TypeAdapter
```

The model stored in Hive is placed under `lib/product/model/` with `@HiveType` + `@HiveField` annotations (see [model_rules.md](model_rules.md)).

After creating a new cache, register it in `HiveService` so it is initialized at startup via the locator.

## Decision tree

| Data type | Method | Location |
|---|---|---|
| Simple key-value (bool, int, String) | SharedPreferences mixin | `product/service/shared_preferences/mixins/` |
| Small settings / flags | SharedPreferences mixin | `product/service/shared_preferences/mixins/` |
| Large lists, model collections | Hive CE | `product/cache/<name>/` |
| Offline-first cached data | Hive CE | `product/cache/<name>/` |
