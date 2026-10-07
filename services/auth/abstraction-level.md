# Abstraction level

Секция описывает auth как сервис целиком: gRPC-поверхность, схему БД, топики, use-cases, env, wiring, тесты.

Не углубляется в реализацию общих библиотек [shared](../../shared/purpose.md) и в protobuf-исходники [proto](../../proto/purpose.md) — там только контракт и менеджеры.

Шаблон сервиса (слои, lifecycle) уже описан на уровне [services/](../purpose.md) и здесь не повторяется.
