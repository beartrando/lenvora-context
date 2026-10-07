# Entry points

`@shared/logger` → `exports["."]` → `dist/index.js` / `dist/index.d.ts`:

- default export — объект `logger`;
- именованный экспорт `logger` (тот же объект);
- API объекта: `log(...args: unknown[])`, `info(...args: unknown[])`, `warn(...args: unknown[])`, `error(...args: unknown[])`.

Больше пакет ничего не экспортирует (нет ни типов, ни субпутей).
