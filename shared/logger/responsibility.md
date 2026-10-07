# Responsibility

- Единый формат консольного вывода: `[ISO-timestamp]`, аргументы, затем `[file:line]` вызывающего кода (парсится 4-я строка стека `Error.stack`, при неудаче — `unknown`).
- 4 метода `log` / `info` / `warn` / `error` — идентичная реализация, уровень на вывод не влияет (все через `console.log`).
- Нет состояния и side-effectов при импорте: модуль только объявляет объект `logger`.
- Точка подмены логов в тестах сервисов: `jest.mock('@shared/logger')` в `jest.setup.ts` (auth, remy, gateway, profile) и в spec-файлах (gateway).
