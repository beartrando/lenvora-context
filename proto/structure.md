# Structure

```text
proto/
├── auth.proto
├── gusteau.proto
├── orchestration.proto
├── profile.proto
├── remy.proto
├── speech-to-text.proto
├── streaming.proto
├── common/
│   ├── empty.proto
│   ├── error.proto
│   ├── health.proto
│   ├── key_value.proto
│   ├── ping.proto
│   └── response.proto
└── .gitignore
```

Каждый файл — `syntax = "proto3"`; у `<service>.proto` `package` = имя сервиса, у `common/*` — `common.<имя>`; сервисные файлы импортируют общие типы из `common/`.
