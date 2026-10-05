# Structure

```
shared/logger/src/
├── index.ts              # единственный исходник
├── index.js, index.d.ts, index.d.ts.map   # устаревшие скомпилированные артефакты,
│                                          # лежащие в src (не покрыты .gitignore)
└── ../dist/              # результат сборки (в .gitignore)
```
