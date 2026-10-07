# Abstraction level

Библиотечный уровень: код, который только импортируется — без собственного процесса, портов, окружения и composition root.

Ниже — сторонние пакеты (`kafkajs`, `pg-boss`, `@grpc/grpc-js`, `@bufbuild/protobuf`); выше — [services](../services/purpose.md), которые собирают из библиотек сервисные обвязки: `infrastructure/message` (Kafka), `infrastructure/job` (pg-boss + мост в Kafka), `infrastructure/grpc/clients` (gRPC), обработчики ошибок.
