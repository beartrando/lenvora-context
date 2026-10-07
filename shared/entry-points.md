# Entry points

Точка входа каждой библиотеки — единственный корневой экспорт `@shared/<name>` (`exports["."]` → `dist/index.js` + `dist/index.d.ts`). Subpath-экспортов (`./foo`) нет:

- [logger](logger/purpose.md) — default и именованный `logger`;
- [errors](errors/purpose.md) — именованные классы ошибок (10 экспортов);
- [kafka-manager](kafka-manager/purpose.md) — функции и типы Kafka;
- [grpc-client-manager](grpc-client-manager/purpose.md) — `GrpcClientManager` и его типы;
- [pg-boss-manager](pg-boss-manager/purpose.md) — default-синглтон `pgBossManager` + типы.

Детали экспорт-точек — в entry-points соответствующей библиотеки.
