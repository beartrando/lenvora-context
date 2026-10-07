# Structure

```text
logger/
├── src/
│   └── index.ts      — getCallerLocation(), getTimestamp(), объект logger, default export
├── dist/             — выход tsc: index.js, index.d.ts, index.js.map
├── package.json      — @shared/logger, exports["."], скрипты build/dev/prepare
├── tsconfig.json     — общий шаблон (ES2022, NodeNext, strict, rootDir src, outDir dist)
├── README.md         — usage + порядок релиза
└── .gitignore        — node_modules, dist, *.tsbuildinfo
```

Одна точка входа, один файл исходников: отдельных модулей и `src/internal/` нет.
