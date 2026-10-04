# @shared/logger — консольный логгер

## Purpose

Минимальный логгер: `log/info/warn/error` в консоль с ISO-таймстампом и позицией
вызова (строка из стека). Default export — синглтон.

## Abstraction level

Лист дерева: нижний уровень [shared/](../CONTEXT.md). Самый маленький пакет уровня
(один файл).

## Parent

[../../shared/CONTEXT.md](../../shared/CONTEXT.md).

## Children

Нет.

## Responsibility

- `logger` (и default export) с четырьмя методами вывода.

## Non-responsibility

- Уровней логирования, файлового вывода, структурированного JSON, транспортов,
  трейсинга, метрик — ничего из этого нет.

## Dependencies

Нет (только devDep `typescript`).

## Dependency direction

Узел без зависимостей внутри проекта. Потребители: `kafka-manager`,
`grpc-client-manager`, `pg-boss-manager` и все сервисы (`import logger from "@shared/logger"`).

## Entry points

`src/index.ts` (named export `logger` + default export).

## Structure

```
shared/logger/src/
├── index.ts              # единственный исходник
├── index.js, index.d.ts, index.d.ts.map   # устаревшие скомпилированные артефакты,
│                                          # лежащие в src (не покрыты .gitignore)
└── ../dist/              # результат сборки (в .gitignore)
```

## Architectural rules

- В `jest.setup.ts` сервисов пакет замокан — тесты не зависят от вывода логгера.
- Наличие `src/index.js` рядом с `index.ts` — фактическое состояние (артефакт),
  при импорте через `main: dist/index.js` не участвует.

## Important files

- `src/index.ts`.

## Navigation

- Нет задач, специфичных для этого уровня, кроме изменения самого логгера.
