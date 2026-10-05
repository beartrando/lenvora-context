# Structure

```
services/front/
├── lib/
│   ├── contracts/proto/     # сгенерированный Dart из proto/
│   ├── domain/              # profile, setting
│   ├── features/            # home, profile, setting, update (UI + feature-инфраструктура)
│   ├── infrastructure/      # app, auth, device, error, http, localization,
│   │                        # proto, storage, websocket
│   ├── shared/              # общие компоненты
│   └── main.dart, app.dart, router.dart, theme.dart, firebase_options.dart
├── env/                     # .env + generate.dart
├── web/ android/ ios/ linux/ macos/ windows/   # платформенные целы
├── assets/, test/, build/, .dart_tool/
├── Makefile, pubspec.yaml, firebase.json
└── .github/workflows/deploy.yml
```
