# Non-responsibility

- Не содержит бэкенд-логики и не переиспользует `@shared/*`; контракты получает из
  proto через Dart-codegen (`protoc --dart_out=grpc:lib/contracts/proto`).
