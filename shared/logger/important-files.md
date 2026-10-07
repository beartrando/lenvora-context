# Important files

- `src/index.ts` — вся реализация: `getCallerLocation()` (парсинг `Error.stack`), `getTimestamp()`, объект `logger`, default export.
- `package.json` — `exports["."]`, скрипты `build`/`dev`/`prepare` (`tsc`), `files: ["dist"]`.
- `tsconfig.json` — эталон компиляции, повторяемый остальными библиотеками.
- `README.md` — usage и порядок релиза (bump версии → git-тег → обновление `services/*/package.json`).
- `dist/index.js` — собираемый артефакт, из него читают потребители.
