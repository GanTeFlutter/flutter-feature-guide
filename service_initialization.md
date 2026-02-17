# Service Initialization & Locator

All services that require initialization follow this flow:

```
ApplicationInit.start()
  -> WidgetsFlutterBinding.ensureInitialized()
  -> Firebase.initializeApp(...)
  -> setupLocator()
      -> _registerSingletons()    // Register all services
      -> _initializeServices()    // Call init() on services that need it
```

## ApplicationInit (`lib/product/init/application_init.dart`)

```dart
import 'package:firebase_core/firebase_core.dart';
import 'package:flutter/material.dart';
import 'package:<project_name>/product/firebase_options.dart';
import 'package:<project_name>/product/service/service_locator.dart';

@immutable
final class ApplicationInit {
  const ApplicationInit();

  Future<void> start() async {
    WidgetsFlutterBinding.ensureInitialized();
    await Firebase.initializeApp(
      options: DefaultFirebaseOptions.currentPlatform,
    );
    await setupLocator();
  }
}
```

## Service Locator (`lib/product/service/service_locator.dart`)

```dart
import 'package:get_it/get_it.dart';

final GetIt locator = GetIt.instance;

Future<void> setupLocator() async {
  _registerSingletons();
  await _initializeServices();
}

void _registerSingletons() {
  locator
    ..registerSingleton<SharedPreferencesService>(
      SharedPreferencesService.instance,
    )
    ..registerSingleton<AudioService>(
      AudioService.instance,
    )
    ..registerSingleton<RemoteConfigService>(
      RemoteConfigService.instance,
    )
    ..registerSingleton<HiveService>(
      HiveService.instance,
    );
}

Future<void> _initializeServices() async {
  await locator<SharedPreferencesService>().init();
  await locator<HiveService>().init();
  await locator<RemoteConfigService>().init();
}

extension ServiceLocator on GetIt {
  SharedPreferencesService get spService => locator<SharedPreferencesService>();
  AudioService get audioService => locator<AudioService>();
  RemoteConfigService get remoteConfigService => locator<RemoteConfigService>();
  HiveService get hiveService => locator<HiveService>();
}
```

## Adding a new service to the locator

1. Add to `_registerSingletons()`:
   ```dart
   ..registerSingleton<NewService>(NewService.instance)
   ```
2. If the service requires async initialization, add to `_initializeServices()`:
   ```dart
   await locator<NewService>().init();
   ```
3. Add a getter to the `ServiceLocator` extension:
   ```dart
   NewService get newService => locator<NewService>();
   ```

## Init pattern

Services that need async setup follow this pattern:

```dart
class SomeService {
  SomeService._();
  static final SomeService instance = SomeService._();

  bool _initialized = false;

  Future<void> init() async {
    if (_initialized) return;
    // setup logic...
    _initialized = true;
  }
}
```
