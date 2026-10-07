# Responsibility

- Переиспользование инфраструктурной обвязки (логирование, доменные ошибки, Kafka, gRPC-клиенты, очередь pg-boss) вместо копирования кода в каждый сервис.
- Единый шаблон пакета, фактически повторяемый всеми 5 библиотеками:
  - `package.json`: `name: @shared/<name>`, `version: 1.0.0`, `main`/`types` → `dist/index.*`, `exports["."]`, `files: ["dist"]`, скрипты `build`/`dev`/`prepare` = `tsc`;
  - `tsconfig.json`: идентичен у всех (ES2022, NodeNext, strict, declaration, sourceMap, `rootDir: src`, `outDir: dist`) — дублирует `tsconfig.base.json`, не наследует его;
  - `README.md` c usage и секцией Release (bump semver → git-тег → обновление потребителей).
- Тестов в самом `shared/` нет: тестирование и моки (`jest.mock('@shared/*')`) — в потребителях `services/*` (jest).
- Потребление: `services/*` объявляют `"@shared/<name>": "workspace:*"`; корневой `pnpm-workspace.yaml` принудительно резолвит их в локальные workspace.
